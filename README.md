# De-bloating a Samsung TV: what works and what doesn't

## Bottom line

The "hack your TV, uninstall the bloat, swap in FLauncher" videos are about
**Android TV / Google TV** devices (Sony, TCL, Hisense, Philips, Chromecast
with Google TV, Nvidia Shield, etc.). Samsung TVs run **Tizen**, Samsung's own
Linux-based OS. Tizen doesn't use ADB or Android packages and can't run
FLauncher, so none of those steps work on a Samsung.

On a Samsung you can:

| Goal | Possible on Samsung? |
|---|---|
| Uninstall Samsung's own apps (TV Plus, Gaming Hub, SmartThings, Art Store, Bixby, etc.) | **No**, not by any supported or reliable method |
| Replace the home screen / launcher | **No**. The home UI is part of the firmware |
| Delete *some* preinstalled third-party apps | **Sometimes**. Some can be deleted; Samsung's own ones are greyed out |
| Stop the viewing tracking (ACR) and targeted ads | **Yes**, with settings, plus network blocking for extra insurance |
| Stop TV Plus / Smart Hub from launching on power-on | **Yes**, on most models |
| Ad-free YouTube | **Often**, with TizenBrew + TizenTube (2017+ models; newer models are hit-and-miss) |
| Get the exact "clean launcher, core apps only" experience | **Yes, but not on the Samsung itself**: cut the TV off the internet and use a streaming box you control |

Start by finding your model number. It's on the sticker on the back, or under
Settings → Support → About This TV. The letter after the series number gives
the year: S90**C** = 2023 (Tizen 7), S90**D** = 2024 (Tizen 8), S90**F** = 2025
(Tizen 9). Samsung skipped "E". Menu names below move around between model
years, so treat the paths as approximate.

---

## Tier 1: Settings (10 minutes, no risk)

### 1. Turn off tracking and ad targeting
Path on 2020+ models (wording varies):
**Settings → All Settings → General & Privacy → Terms & Privacy** (on some
firmware it's labelled **Privacy Choices**).

Turn **off** / un-tick:
- **Viewing Information Services**. This is ACR, which fingerprints what's on
  the screen.
- **Interest-Based Advertisements** (sometimes "Interest-Based Ads Service").
- **Voice Recognition Services** / Bixby voice data, if you don't use voice.
- Any marketing or "personalised content" consent you don't want.

On pre-2020 models, look under **Settings → Support → Terms & Policy** and turn
off **SyncPlus and Marketing** and Viewing Information Services.

If the TV is signed into a Samsung account, the same choices are also in
Samsung's online Privacy dashboard.

Why this matters: a 2024 peer-reviewed study (Anselmi, Vekaria et al., ACM
IMC '24) found that Samsung's and LG's ACR was **on by default**. It kept
working **even when the TV was only a "dumb" display for an HDMI device**, so a
Roku or Apple TV plugged into it doesn't escape it. **Opting out did stop the
ACR network traffic.** The settings therefore do real work. Check them again
after firmware updates.

### 2. Stop the auto-launch junk
**Settings → General → Smart Features** (on newer firmware it may be under
General & Privacy). Turn **off**:
- **Autorun Samsung TV Plus**
- **Autorun Smart Hub**
- **Autorun Last App** (unless you want it)

Then power-cycle the TV completely: hold the remote's power button until it
restarts.

Samsung TV Plus can't be uninstalled, but on many models you can highlight it
on the home bar, long-press OK, and choose **Remove → Disable**.

### 3. Delete what can be deleted
**Apps → Settings (gear icon)**: select an app → **Delete**. On 2022+ models,
**Settings → Support → Device Care → Manage Storage** also works. Third-party
preinstalls such as some streaming apps can often be deleted. Samsung's own
apps show **Delete greyed out**, and that's by design.

---

## Tier 2: Network-level blocking (moderate effort, low risk)

### Option A: Cut it off from the internet completely (strongest)
Block the TV in your router's device list, or forget the Wi-Fi network on the
TV and unplug Ethernet. Then plug in a streaming box (Tier 4). You lose:
- Firmware updates over the air. You can still update by USB with a file from
  Samsung's support site for your model.
- The TV's built-in apps. That's the point.

HDMI-CEC (Samsung calls it **Anynet+**) keeps working offline, so the box's
remote can still power the TV on and off and control volume.

### Option B: Keep it online but filter it with DNS
Use **Pi-hole** or **AdGuard Home** on your network, or **NextDNS** if you
don't want extra hardware. Its *Privacy → Native Tracking Protection* section
has a Samsung option.

Blocklist maintained for smart TVs:
- Pi-hole: `https://perflyst.github.io/PiHoleBlocklist/SmartTV.txt`
- AdGuard Home: `https://perflyst.github.io/PiHoleBlocklist/SmartTV-AGH.txt`

The Samsung section includes the ad and ACR domains (`samsungads.com`,
`samsungadhub.com`, `samsungacr.com`, `log-*.samsungacr.com`, ...). Be aware
that it **also** blocks:
- `www.samsungotn.net`, which carries firmware updates, and
- `samsungcloudsolution.com/.net` and `samsungqbe.com`, which the Smart Hub
  and app store depend on.

Expect some breakage. Whitelist entries one at a time if something you
actually use stops working. The list's own maintainers have already commented
out some Samsung entries because of "numerous connection problems".

**Don't trust the TV to use your DNS.** Some smart TVs fall back to hardcoded
resolvers such as 8.8.8.8, and there are reports of Samsung sets doing this.
On your router or firewall, for the TV's IP address:
1. Redirect (NAT) all outbound port 53 traffic (UDP and TCP) to your
   Pi-hole/AdGuard box.
2. Block outbound port 853 (DNS-over-TLS).
3. Block outbound port 443 to the big public DNS-over-HTTPS resolvers
   (`8.8.8.8`, `8.8.4.4`, `1.1.1.1`, `1.0.0.1`, `9.9.9.9`,
   `149.112.112.112`).

---

## Tier 3: Developer mode + TizenBrew (ad-free YouTube, optional)

[TizenBrew](https://github.com/reisxd/TizenBrew) loads community mods on Tizen
3.0+ (2017 or newer). The popular one is **TizenTube**, an ad-free YouTube
with SponsorBlock-style skipping. It **doesn't** remove system apps or change
the launcher. It only adds an app.

1. Open **Apps** on the TV and type **1 2 3 4 5** on the remote or on-screen
   keypad. The Developer Mode dialog appears.
2. Switch Developer Mode **On**, enter your PC's local IP address, and reboot
   the TV. "Develop Mode" now shows at the top of the Apps screen.
3. On the PC, run **TizenBrew Installer** (desktop app) and click *Install
   TizenBrew*. On Tizen 7+ (2023+) it makes you sign in to a Samsung account
   to create a signing certificate.
4. In TizenBrew's module manager, add `@foxreis/tizentube`.

Caveats:
- Developer Mode has to **stay on** for TizenTube to keep working.
- Users report install failures on **2024+ (Tizen 8+)** sets: certificate and
  signature errors, and a "TV is too new" error with the browser installer.
- YouTube actively fights ad-blockers, so expect breakage and updates.

---

## Tier 4: The real answer if you want "core apps only + a clean launcher"

Make the Samsung a dumb panel (Tier 2, Option A) and put the smarts in a box
you control.

- **No tinkering, fewest ads:** Apple TV 4K. There's nothing to debloat.
- **Exactly what the video showed (ADB debloat + FLauncher / Projectivy):** a
  Google TV or Android TV box: Google TV Streamer, Walmart Onn 4K Pro, or
  Nvidia Shield TV.

### Google TV / Android TV debloat, condensed
On the box:
1. **Settings → System → About**, then click **Android TV OS build** 7 times to
   unlock Developer options.
2. **Settings → System → Developer options**, then turn on **USB debugging**.
   On newer builds, use **Wireless debugging → Pair device with pairing code**
   instead.

On a PC with Android platform-tools installed:
```sh
adb connect <box-ip>:5555        # or: adb pair <ip>:<port>  then adb connect
adb shell pm list packages        # see what's installed

# Remove an app for your user (reversible, doesn't touch the system image):
adb shell pm uninstall --user 0 <package.name>
# Put it back:
adb shell cmd package install-existing <package.name>
```
Remove packages **one at a time** and test after each one. Only remove things
you can positively identify. Don't touch Google Play Services
(`com.google.android.gms`), settings, setup, remote/input or `com.android.*`
system packages.

### Swapping the launcher (FLauncher)
**Install the new launcher first.** Disabling the stock one with no
replacement can leave you with no home screen.

- **Safest:** install FLauncher (or Projectivy Launcher) and use **Button
  Mapper** to remap the remote's Home button to it.
- **Full replacement** (from the FLauncher README, *tested only on Chromecast
  with Google TV*):
  ```sh
  adb shell pm disable-user --user 0 com.google.android.apps.tv.launcherx
  adb shell pm disable-user --user 0 com.google.android.tungsten.setupwraith
  # undo:
  adb shell pm enable com.google.android.apps.tv.launcherx
  adb shell pm enable com.google.android.tungsten.setupwraith
  ```
  One user reports this breaks the remote's YouTube button. Older Android TV
  (non-Google-TV) builds such as the Shield use `com.google.android.tvlauncher`
  instead.

---

## Not recommended: shell or root on the Samsung itself

What exists publicly, and why it isn't worth it:
- With Developer Mode on and Tizen Studio's `sdb`, the SamyGO forum documents
  an undocumented `vd_applist` / `vd_appuninstall` route for removing
  preinstalled apps. It works on some firmware for some (mostly third-party)
  apps. A user in April 2024 reported it simply returning "can not uninstall
  app". On many retail sets, `sdb shell` just returns `closed`.
- Scattered forum posts describe root on specific firmware builds, for example
  a 2023 S90C. Even with root, Samsung's SMACK mandatory access control,
  **UEP** (Unauthorized Execution Prevention) and a read-only system partition
  still block real changes. Firmware updates close these holes. There's no
  stock recovery to fall back on, so a mistake can brick a TV that costs far
  more than a $50–$100 streaming box.

Not worth it, given that Tiers 1, 2 and 4 get you about 95% of the result with
no risk.

---

## Sources
- TizenBrew docs: https://github.com/reisxd/TizenBrew/blob/main/docs/README.md
- FLauncher README (launcher-disable commands, Button Mapper method): https://github.com/svrooij/flauncher
- Smart TV blocklist: https://github.com/Perflyst/PiHoleBlocklist
- Samsung TV developer mode: https://developer.samsung.com/smarttv/develop/getting-started/using-sdk/tv-device.html
- Samsung: some preinstalled apps can't be removed: https://www.samsung.com/in/support/tv-audio-video/how-to-uninstall-applications-from-smart-tv/
- Samsung: Smart Hub autorun toggle: https://www.samsung.com/ph/support/tv-audio-video/how-to-activate-or-deactivate-the-smart-hub/
- SamyGO thread on `vd_appuninstall`: https://forum.samygo.tv/viewtopic.php?t=14101
- XDA thread on Samsung TV root: https://xdaforums.com/t/samsung-smart-tv-jailbreak-root-etc.4221417/
- ACR study (IMC '24): https://arxiv.org/abs/2409.06203
- RTINGS smart-TV tracking tests: https://www.rtings.com/tv/learn/research/smart-tv-tracking
- Stopping TV Plus autorun: https://www.tomsguide.com/tvs/you-can-stop-samsung-tv-plus-from-playing-automatically-heres-how
- Forcing DNS through Pi-hole: https://labzilla.io/blog/force-dns-pihole

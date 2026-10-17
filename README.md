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

## Your TV: The Frame Pro 75" (QN75LS03FWFXZA, 2025)

This is a 2025 model, so it runs the newest and most locked-down version of
Tizen. There's no root and no way to swap the launcher. TizenBrew is
hit-and-miss on 2024+ models. Even if you could get in, nothing in there would
make it faster: there's no hidden "speed" or overclock setting. The slowness
comes from three places, and each has its own fix.

### 1. Menus and remote feel sluggish
Reviewers flagged this on the Frame Pro: Trusted Reviews lists a "laggy,
stuttering interface with Wireless One Connect" as a con, and owners report
the remote not responding. Work through these in order:
1. **Update the firmware:** Settings → All Settings → Support → Software
   Update → Update Now. Turn on Auto Update. Reviewers say this is the first
   thing to try for interface speed.
2. **Do a real cold reboot.** A normal press of the power button never
   restarts the TV; it only switches between Art Mode and standby. Unplug
   **both** the TV and the Wireless One Connect box from the wall for 60
   seconds. Owners report this gives relief, though for some it only lasts a
   few days.
3. **Run Device Care:** Settings → All Settings → Support → Device Care →
   Start Device Care. Samsung says it clears cached memory and closes
   background apps. Then go to **Manage Storage**, pick any slow app, and
   choose **Clear cache**. Don't choose Clear data, which signs you out.
4. **Cut background load.** Follow Tier 1 below: turn off Viewing Information
   Services (it constantly fingerprints what's on screen), turn off all three
   Autorun options, and delete apps you don't use. If you don't talk to the
   TV, also turn off voice wake-up (Settings → General → Voice).
5. **Re-pair the remote** if presses get dropped: point it at the TV and hold
   **Back + Play/Pause** together for about 3 seconds.
6. **Last resort:** a factory reset (Settings → All Settings → General &
   Privacy → Reset; the default PIN is 0000). After that, set up only what you
   use.

If it's still bad after all of that, it's a defect or design flaw, not a
settings problem. The TV is under Samsung's warranty, so use it, or exchange
it if you're still inside the store's return window.

### 2. Devices plugged into HDMI feel delayed (console, PC, cable box)
Everything plugged into the **Wireless One Connect box** travels to the TV
over a wireless link. Measured input lag through the box was about 35–36 ms
at 4K60 (Tom's Guide, TechRadar), against about 9–14 ms through the TV's own
port (Choose.tv).

- The TV panel has its own **Micro-HDMI 2.1 port** on the back. Plug your
  console or PC in there with a certified Ultra High Speed Micro-HDMI-to-HDMI
  cable. That bypasses the wireless link completely. On the 2025 model this
  port has **no eARC**, so a soundbar still goes on the box.
- For everything else, keep the box within about 10 m with a clear line of
  sight to the TV. Not in a closed cabinet, not behind metal. Samsung warns
  that walls and obstructions hurt the link.
- For games, turn on Game Mode and Auto Game Mode (ALLM).

### 3. Apps are slow to open, scroll, or start playing
This is the TV's processor and Tizen's apps, and no setting fixes it
properly. The real fix is a **streaming box**. The current Apple TV 4K is the
fastest and cleanest. A Google TV Streamer works if you want the debloat +
FLauncher route from Tier 4. Plug the box into the TV's Micro-HDMI port to
skip the wireless hop too, or into the One Connect box if the TV's port is
taken by a console. Art Mode keeps working either way. The TV's built-in
processor is fixed for its whole life, while a streaming box is cheap to
replace when it eventually gets slow.

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
- Frame Pro review (laggy interface with Wireless One Connect): https://www.trustedreviews.com/reviews/samsung-frame-pro
- Frame Pro review (input lag over the wireless box): https://tomsguide.com/tvs/samsung-frame-pro-tv-2025-review
- Frame Pro review (wired Micro-HDMI vs wireless input lag): https://www.choose.tv/us/tv/reviews/samsung/samsung-the-frame-pro_AGM
- TechRadar on Wireless One Connect input lag: https://www.techradar.com/televisions/i-tested-samsungs-new-wireless-tv-connection-box-and-gamers-need-to-know-one-thing
- RTINGS Frame Pro review: https://www.rtings.com/tv/reviews/samsung/the-frame-pro-2025
- Best Buy Q&A (no wired box link; Micro-HDMI on the TV): https://www.bestbuy.com/site/questions/samsung-65-class-ls03fw-the-frame-pro-series-neo-qled-4k-uhd-art-mode-samsungvision-ai-smart-tv-w-wall-mount-2025/6613680/question/6c18c7d5-153a-3655-a507-f339760af709
- Samsung Device Care: https://www.samsung.com/uk/support/tv-audio-video/how-to-device-care
- Owner reports of Frame Pro lag: https://us.community.samsung.com/t5/QLED-and-The-Frame-TVs/2025-Frame-Remote-Stops-Working/td-p/3248647

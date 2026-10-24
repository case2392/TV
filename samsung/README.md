# Samsung The Frame: fewer ads, less tracking, less slowness (no box needed)

## The honest answer first
Your TCL runs Google TV (Android). That's why we could connect with ADB,
switch off apps and replace the home screen. **The Frame runs Samsung's own
system, Tizen, which has none of that:**

- **No way to remove Samsung's apps or bloat.** Samsung's nearest thing to ADB
  is Developer Mode with a tool called `sdb`, but on retail TVs it gives no
  command line to switch Samsung's apps off with. The only route that actually
  strips them is rooting the TV with a security exploit. That's the one that
  has left TVs on unrecoverable "developer" firmware needing a paid repair, so
  **don't go there.** (Developer Mode by itself, used only to install your own
  signed app as below, is the normal supported path and is fine.)
- **No safe replacement of the Home button.** You *can* install a custom
  home-screen app (see "A custom home-screen app" below), but nothing can
  reliably make it open when you press **Home** without the same exploit above,
  and such an app removes no ads either way. Tizen has nothing like Projectivy
  or the tv-vibehack launcher.

What **does** work is completely safe and can all be undone:

1. **Settings** that cut tracking and stop the TV opening its ad-filled home
   screen.
2. **Ad-blocking DNS.** This is the one thing that actually removes the
   sponsored tiles from the home screen. You type two numbers into the TV, and
   it costs nothing.
3. **Speed clean-up:** a proper restart, clearing caches, and resets.

(A cleaner-looking home-screen *app* can also be installed, but it's cosmetic
and removes no ads — see "A custom home-screen app" near the end.)

Menus move between model years. Paths below cover 2025 (Frame Pro LS03FW,
Tizen 9), 2023–24 (LS03C/D) and older. If an item isn't where it says, look in
the nearest-named menu.

---

## Step 0: Two quick checks (1 minute)
- **Model:** Settings → Support → About This TV. `LS03F`/`LS03FW` = 2025,
  `LS03D` = 2024, `LS03C` = 2023, `LS03B` = 2022.
- **Usage Mode must be "Home Mode":** Settings → All Settings → General &
  Privacy → System Manager → **Usage Mode**. Retail/Store Demo mode loops ads
  and keeps resetting your settings. That would make everything below seem
  not to work.

## Step 1: Turn off tracking (5 minutes)
**Settings → All Settings → General & Privacy → Terms & Privacy** (sometimes
labelled **Privacy Choices**):
- **Viewing Information Services: OFF.** This is the screen-watching tracker
  (ACR). A 2024 university study found it runs even when you're just using
  the TV as a screen for an HDMI device, and that switching it off does stop
  it.
- **Interest-Based Advertising: OFF.**
- Untick marketing consents you don't want (SyncPlus/marketing on older
  sets).

Also: **Settings → General & Privacy → Voice → Bixby Voice Settings → Voice
Wake-up: OFF** (stops it always listening). The remote's mic button still
works.

> These stop tracking and *personalised* ads. They do **not** remove the ad
> tiles themselves; Step 3 does that.

## Step 2: Stop landing on the ad screen (5 minutes)
- **Autorun Smart Hub: OFF**, so the TV turns on to your last input instead of
  the home screen.
  - 2025: Settings → All Settings → **Advanced Features**
  - 2023–2024: Settings → All Settings → General & Privacy → **Start Screen
    Options**
  - 2022 and older: Settings → General → **Smart Features**

  While you're there, decide about **Autorun Last App** (reopens whatever you
  were last using).
- **Samsung TV Plus:** on the home screen, highlight it → press and hold
  **OK** → **Remove** → choose **Disable** (not just Remove). This removes the
  tile and its channels and stops it running. You can re-add it later.
- **Clear out other tiles:** highlight any app → press and hold OK (or press
  Down) → **Remove**. It only comes off the home row; it isn't deleted.
- **Personalisation:** if your Smart Hub settings have a "personalisation" or
  "recommendations" switch, turn it off. Newer firmware may not have one, and
  Samsung says there's no setting that removes the generic promo banner. That's
  why Step 3 exists.

## Step 3: Block Samsung's ad servers (the real ad-killer, 5 minutes)
The sponsored tiles are downloaded from Samsung's ad servers. If the TV can't
look those servers up, the ads can't load. This can't damage the TV, and
undoing it takes ten seconds.

### Option A: on the TV only (easiest, free, no account)
1. **Settings → All Settings → Connection** (older: **General**) → **Network
   → Network Status**. Wait for the test, then **IP Settings**.
2. **DNS Setting → Enter manually**, and enter **94.140.14.14**. If it asks
   for a second DNS, enter **94.140.15.15**.
3. Select **OK**, then turn the TV fully off and on (see Step 4).

That's AdGuard's free public ad-blocking DNS. Its live filter (checked
October 2026):
- **Blocks:** Samsung's ad servers (`samsungads.com`, the `adgrx.com` ad
  exchange, `samsungtvads.com`), the screen-tracking servers (`samsungacr.com`,
  `acr-*-prd.samsungcloud.tv`) and Samsung telemetry (`nmetrics.samsung.com`,
  `analytics.samsungknox.com`, …).
- **Does not touch:** Samsung's firmware-update, app-store, account-login or
  TV Plus servers. Your TV keeps working normally.

**Undo:** the same screen → DNS Setting → **Get automatically**.

### Option B: the whole house, customisable (optional)
If you'd rather set it once on your router, or want to control exactly what's
blocked, use NextDNS (nextdns.io, free tier):
- Turn on Privacy → Native Tracking Protection → **Samsung**, plus its ad
  lists.
- Add the domains below to the Denylist.
- Put NextDNS's addresses in your **router's** DNS settings. On the TV itself,
  NextDNS needs your home IP "linked" in its dashboard, so the router is
  simpler.

Block (each entry covers its subdomains in NextDNS/AdGuard):
```
samsungads.com
samsungadhub.com
adgrx.com
samsungtvads.com
samsungacr.com
nmetrics.samsung.com
insights.samsung.com
bigdata.ssp.samsung.com
analytics.samsungknox.com
```
**Never block** these. They carry firmware updates, the app store, account
login, the clock, and TV Plus:
```
samsungotn.net             otnprd8/9/10/11.samsungcloudsolution.net
otn.samsungcloudcdn.com    cdn.samsungcloudsolution.com
lcprd1.samsungcloudsolution.net   auth.samsungosp.com
time.samsungcloudsolution.com     osb-ussvc.samsungqbe.com (TV Plus)
```
If you use the popular Perflyst "SmartTV" Pi-hole list instead:
- It hasn't been updated since July 2023.
- It blocks firmware updates (`www.samsungotn.net`) and parts of Samsung's
  cloud.
- Allow those back if updates or apps stop working.

### What blocking won't remove
- Ads **inside** apps (YouTube, Tubi, Samsung TV Plus channels). They come from
  the same servers as the video.
- On some sets, a generic "recommended" row that isn't served as an ad.

It may also make the home screen feel quicker, because it no longer waits on
ad downloads. That's commonly reported, not measured.

## Step 4: Make it faster (10–20 minutes)
Try these in order, and stop when it's fast enough:
1. **Real restart.** On the Frame, the power button only switches between Art
   Mode and off; the TV never actually reboots. Unplug the TV **and** the One
   Connect box (Frame Pro: both power cords) for 60 seconds.
2. **Device Care:** Settings → Support → **Device Care** (sometimes "Self
   Care") → **Start Device Care**. Then **Manage Storage** → pick any slow
   app → **Clear cache**. Not "Clear data", which signs you out.
3. **Update once:** Settings → Support → Software Update → Update Now.
4. **Reset Smart Hub:** Settings → Support → Device Care → **Reset Smart
   Hub**. It clears apps and their caches but keeps your TV settings. Do it
   from an HDMI input or live TV, not inside an app. You'll sign back into your
   streaming apps.
5. **Factory reset, last resort:** Settings → All Settings → General &
   Privacy → **Reset**. The PIN is 0000 on older sets; 2025 sets use the PIN
   you created. During setup again:
   - accept only the **required** terms and untick every optional consent
     (viewing information, interest-based ads, marketing, voice)
   - the remote may need re-pairing
   - then redo Steps 1–3

If a console or cable box plugged into a **Frame Pro** feels laggy, that's the
wireless One Connect box (about 35 ms of delay in reviews). The panel has its
own Micro-HDMI port that skips it.

## After any software update
Updates have been reported to switch some of these settings back on. Spend
two minutes re-checking Step 1 and Autorun. The DNS setting from Step 3 isn't
affected by Samsung's toggles.

## A custom home-screen app (optional, cosmetic only)
A few projects build a cleaner home-screen *app* for the Frame: **CleanFrame**
(tested on a 2024 Frame) and **Free My TV** (Samsung 2017+). You install them
with Samsung's own developer tools and your own signing certificate, which is
safe and reversible. Be clear about what they are:
- They **remove no ads and disable nothing.** Samsung's home screen stays.
- Neither has been tested on a 2025 **Frame Pro** like yours.
- They often **won't open on their own** ("Autorun Last App" is reported not to
  work for apps you install yourself), so you'd open it by hand like any app.
- Their trick for taking over the **Home button** uses what looks like a Tizen
  security hole Samsung is patching, and the author warns the undo may fail
  after an update. **Skip that part.**

So an app like this is a nicer-looking launcher you open yourself, not an
ad-remover. The ad/tracking work above is still what does the real job.

## Not worth doing
- **Rooting the TV** (`samsung-tv-root`, `couchroot`, or any "uninstall Samsung
  apps" exploit). This is the only thing that truly debloats Tizen, and it's
  the one with real brick risk on a ~$2000 TV. Not worth it.
- **The Home-button takeover** in CleanFrame/couchroot, for the reasons above.
- **TizenBrew/TizenTube.** Only adds an ad-free YouTube app. Doesn't touch the
  home-screen ads. (It installs on current TVs again since a Sept 2025 fix, but
  it still won't help with ads or bloat.)
- **Reframe.** A 2021 Tizen 6 experiment that also needs a Plex server. Doesn't
  apply to your TV.

---

## Sources
- Samsung: [Autorun / Start Screen Options](https://www.samsung.com/au/support/tv-audio-video/use-autorun-start-screen-options-on-your-samsung-tv/),
  [Smart Hub autorun](https://www.samsung.com/in/support/tv-audio-video/how-to-activate-or-deactivate-the-smart-hub/),
  [TV Plus on 2025 TVs](https://www.samsung.com/sg/support/tv-audio-video/use-tv-plus-on-your-2025-samsung-smart-tv),
  [Device Care](https://www.samsung.com/uk/support/tv-audio-video/how-to-device-care),
  [Reset Smart Hub](https://www.samsung.com/au/support/tv-audio-video/reset-smart-hub/),
  [Retail/demo mode](https://www.samsung.com/us/support/troubleshoot/TSG10002346),
  [Factory reset](https://www.samsung.com/ae/support/tv-audio-video/how-can-i-reset-my-tv-to-factory-settings/)
- Blocklists (read live): [AdGuard DNS filter](https://github.com/AdguardTeam/AdGuardSDNSFilter),
  [AdGuard public DNS](https://adguard-dns.io/en/public-dns.html),
  [Perflyst SmartTV list](https://github.com/Perflyst/PiHoleBlocklist),
  [NextDNS Samsung list](https://github.com/nextdns/native-tracking-domains)
- Tracking: [ACR study, IMC '24](https://arxiv.org/abs/2409.06203),
  [Texas ACR settlement (2026)](https://www.androidauthority.com/samsung-tv-data-collection-settlement-3645703/)
- Why not Developer Mode: [Bishop Fox on retail Tizen through 9.0](https://bishopfox.com/blog/samsung-tizen-os-version-through-9-0),
  [developer firmware 9999.9 reports](https://eu.community.samsung.com/t5/tv/firmware-developer-update-9999-9/m-p/3735450/highlight/true),
  [Developer Mode removed on new TVs](https://eu.community.samsung.com/t5/tv/developer-mode-removed-on-latest-samsung-tvs/td-p/10921837),
  [TizenBrew certificate failures on Tizen 8](https://forum.developer.samsung.com/t/check-certificate-error-invalid-certificate-chain/38782),
  [no custom home screen on Tizen](https://forum.developer.samsung.com/t/develop-custom-home-screen-for-samsung-smart-tv/37592)

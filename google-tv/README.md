# Clean up a TCL Google TV (the tweet's method)

**What you get:**
- TCL's ad service, promo apps and junk switched off.
- Optionally, Google TV's ad-filled home screen replaced with a clean one
  (Projectivy Launcher).
- Everything can be undone.

**What you don't get:**
- Google's own data collection stays.
- Google TV itself stays; this cleans it, it doesn't replace it.
- A big software update can occasionally switch a disabled app back on. If
  that happens, just run the clean-up again.

---

## Step 1: Settings (5 minutes, no hacking)
Menu names vary a little between TCL models.
- **Settings → System → Privacy** (or **Settings → Privacy**):
  - Turn off **Viewing Information Services**, sometimes called "Smart TV
    Experience". This is the feature that tracks what's on screen.
  - Turn off **Usage & diagnostics**.
  - Under **Ads**, opt out of personalised ads and **delete the advertising
    ID**.
  - Under **User Agreements**, withdraw any TCL agreements you don't need.
- **Optional built-in "clean mode":** Settings → Accounts & Sign In → your
  profile → **Apps only mode**. It hides the recommendations. The catch: it
  also turns off Google Assistant and search, your watchlist and library, and
  installing apps. The launcher swap in Step 5 is usually the better trade.

## Step 2: Turn on developer mode on the TV (2 minutes)
1. **Settings → System → About**. Scroll down to **Android TV OS build** and
   press OK on it **7 times** until it says you're a developer.
2. Go back to **Settings → System → Developer options** and turn on **USB
   debugging**.
3. Find the TV's IP address: **Settings → Network & Internet →** your Wi-Fi
   network. It looks like `192.168.1.50`. Write it down.

## Step 3: Set up Claude on your computer (10 minutes)
The computer must be on the **same Wi-Fi** as the TV.
1. Install the **Claude desktop app** from <https://claude.com/download> and
   sign in. Claude Code needs a paid plan: Pro, Max, Team or Enterprise.
2. Download this folder: <https://github.com/case2392/TV/archive/refs/heads/claude/serene-curie-0jdqjw.zip>.
   Unzip it somewhere easy, like your Documents folder.
3. In the Claude app, click the **Code** tab → **Local** → **Select folder**,
   and pick the unzipped folder.

## Step 4: Tell Claude to do it
Type:

> Clean up my TCL Google TV. Its IP address is 192.168.1.50.

Use your own IP address. Claude will:
1. Get the free `adb` tool from Google into that folder. Nothing gets
   installed on your system.
2. Connect to the TV. **A prompt appears on the TV: tick "Always allow from
   this computer" and press OK.**
3. Show you the junk it found and ask before switching anything off. It
   follows the safe list in [`packages.md`](packages.md).
4. Restart the TV and have you check that the Home button, switching
   inputs/HDMI, the remote and voice search still work.

Every change is logged in `google-tv/my-changes.txt`.

## Step 5 (optional): Replace the home screen, like the tweet

### Option A: the tweet's Apple TV-style home screen (free, Claude builds it)
This folder includes [tv-vibehack](tv-vibehack/), the open-source launcher
from the tweet. You get:
- a black screen with a clock
- your four favourite apps in a dock
- the rest of your apps one press below
- Inputs, Wi-Fi and Settings buttons

It has no ads and doesn't use the internet.

Tell Claude:

> Give my TV the Apple TV-style home screen from tv-vibehack.

Claude will:
1. Ask which four apps go in the dock.
2. Download Android's build tools into this folder. That's about 1 GB, and
   takes 10–20 minutes the first time.
3. Build the launcher and install it.
4. Check it with screenshots from the TV.
5. Only then switch off Google's home screen.

Afterwards, test switching HDMI inputs and typing in a search box. Claude will
walk you through it.

### Option B: Projectivy Launcher (ready-made, nothing to build)
1. On the TV, install **Projectivy Launcher** from the Play Store and **open
   it once**.
2. In Projectivy, open its settings (top right) → **General** → turn on
   **Override current launcher**. Now the Home button opens Projectivy.
3. If that doesn't stick (some Google TV updates have broken it), tell Claude:
   *"Switch my TV's home screen to Projectivy for good."*

Either way: once Google's home screen is switched off, the remote's YouTube
button may stop working, and you should check that input switching still
works.

## Undo
- Tell Claude *"Undo everything in my-changes.txt"*, or *"Turn
  com.tcl.whatever back on"* for a single app.
- Worst case, a **factory reset** (Settings → System → About → Reset) restores
  every app and setting.

## Doing it by hand instead
These work the same in Windows PowerShell or a Mac Terminal, once `adb` is
installed:
```
adb connect 192.168.1.50:5555
adb shell pm list packages tcl                                   # see TCL's apps
adb shell pm disable-user --user 0 com.tcl.bootadservice         # switch one off
adb shell pm enable com.tcl.bootadservice                        # switch it back on
adb reboot

# Replace the home screen (only after Projectivy is installed and opened once):
adb shell pm disable-user --user 0 com.google.android.apps.tv.launcherx
adb shell pm disable-user --user 0 com.google.android.tungsten.setupwraith
# Put Google's home screen back:
adb shell pm enable com.google.android.apps.tv.launcherx
adb shell pm enable com.google.android.tungsten.setupwraith
```
Stick to [`packages.md`](packages.md). Some TCL apps, such as
`com.tcl.waterfall.overseas`, have caused TVs to get stuck at the boot logo.

## Bonus: drive it from your phone
In the Claude desktop app's Code session, type `/remote-control`. The session
then shows up in the Claude app on your phone under **Code**, so you can sit
on the couch and watch the TV while Claude works. The computer has to stay on
with the app open.

---
Sources: [Projectivy override setting](https://www.galaxus.de/en/page/a-tv-home-screen-without-ads-projectivy-launcher-spices-up-android-and-google-tv-38294),
[Google TV update affecting Projectivy override](https://www.androidauthority.com/google-tv-update-blocks-projectivy-override-3649445),
[launcher disable commands (FLauncher README)](https://github.com/svrooij/flauncher),
[Apps only mode (TCL)](https://support.tcl.com/en_US/google-tv-apps-only-mode),
[TCL debloat lists](packages.md),
[Claude desktop app](https://code.claude.com/docs/en/desktop-quickstart.md),
[Remote Control](https://code.claude.com/docs/en/remote-control.md).

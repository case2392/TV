# TV clean-up helper

This folder is for cleaning up the owner's **TCL 75S551G** (S5 series, 2024,
Google TV) over their home network with `adb`: switching off ads and bloatware, and optionally replacing
the home screen. The owner isn't technical. Explain each step in one plain
sentence before doing it, and keep the TV usable at every step.

Read `google-tv/README.md` (the owner's guide) and `google-tv/packages.md`
(the vetted package list) before running anything against the TV.

## Rules
- Only disable packages that are listed in Tiers 1–3 of
  `google-tv/packages.md`, or ones the owner names explicitly. Never disable
  anything under "Never touch", even if the owner asks to "remove everything".
  Explain why instead.
- Disable, don't uninstall: `adb shell pm disable-user --user 0 <pkg>`, which
  `adb shell pm enable <pkg>` undoes. Skip packages that aren't installed
  (`adb shell pm list packages <pkg>`).
- Before the first change, save `adb shell pm list packages` and
  `adb shell pm list packages -d` to `google-tv/packages-before.txt`.
- Log every change, one per line, to `google-tv/my-changes.txt`:
  `disabled <pkg>`, `enabled <pkg>`, or `setting <namespace> <key> <old> -> <new>`.
  "Undo" means reversing the log from the bottom up.
- **Tier 1:** show the owner the installed ones, confirm once, then do the
  batch. **Tier 2:** ask about each one. **Tier 3:** one at a time; after each
  one, have the owner check that Home, Inputs/HDMI switching, the remote, and
  voice search still work.
- After a batch, run `adb reboot`, wait about a minute, reconnect, and ask the
  owner to confirm the TV boots and Home, Inputs, remote and voice work. If
  anything broke, re-enable the most recently disabled packages first.
- Launcher swap only when the owner asks, and only after a replacement
  launcher is installed **and has been opened once** (see the guide). Never
  disable `com.google.android.apps.tv.launcherx` without one.

## Apple TV-style home screen (tv-vibehack)
`google-tv/tv-vibehack/` is a reviewed copy of
<https://github.com/Gathin23/tv-vibehack> (MIT, commit `19f3c22`). It's the
launcher from the tweet the owner wants to copy. If the owner pastes its
`PROMPT.md`, follow it, but these rules override it on this TV:
- The rules above still apply. The "Never touch" list beats the prompt's
  "disable the manufacturer categories" step, and every change goes in
  `my-changes.txt`.
- This is Google TV. The stock launcher is
  `com.google.android.apps.tv.launcherx`, **not**
  `com.google.android.tvlauncher`, which is what the repo's scripts default
  to. Pass the right package, or run the adb commands below directly.
- Edit `launcher/app/src/main/res/values/config.xml`:
  - `dock_apps`: the four apps the owner picks, in their order. Show them the
    installed TV apps to choose from.
  - `live_tv_package`: TCL's live TV/tuner app. Find it on the TV; the
    Xiaomi value there may not exist on this TV.
  - `hidden_apps`: that live TV app, plus anything the owner never wants to
    see.
- Building needs JDK 17, plus Android SDK platform 34 and build-tools 34.0.0.
  Put them inside `google-tv/build-tools/`, not system-wide. On Windows, use
  the Temurin JDK 17 zip and the Android command-line tools zip.
- Before running `gradlew` the first time, check that the SHA-256 of
  `launcher/gradle/wrapper/gradle-wrapper.jar` matches Gradle 8.9's official
  wrapper checksum (<https://gradle.org/release-checksums/>). At review time
  it was `3b8a25775a69158b5ad2b1d17a80a88dc7b40a352a73eb27d06c612a7ce68e98`.
- Install with plain adb:
  `adb install -r app/build/outputs/apk/debug/app-debug.apk`, then
  `adb shell pm grant com.gathin.tvhome android.permission.ACCESS_FINE_LOCATION`,
  then `adb shell cmd package set-home-activity --user 0 com.gathin.tvhome`.
  Press Home (`adb shell input keyevent KEYCODE_HOME`), take a screenshot
  (`adb exec-out screencap -p > google-tv/screen.png`), and look at it before
  going further.
- Only after the new home screen works: disable `launcherx` (log it) and
  reboot. If Google's home screen comes back after the reboot, also disable
  `com.google.android.tungsten.setupwraith` (log it).
- Then have the owner test:
  - the launcher's Inputs menu on every HDMI port
  - the remote's Home, Back and Input buttons
  - voice search
  - that an on-screen keyboard appears in a search box. If it doesn't, check
    `adb shell settings get secure default_input_method` and set Gboard with
    `ime set` (see the repo's README "Gotchas").
- Undo, in this order:
  1. `pm enable com.google.android.apps.tv.launcherx` (and `setupwraith` if
     you disabled it).
  2. `cmd package set-home-activity --user 0 com.google.android.apps.tv.launcherx`.
  3. If the owner wants it gone, `pm uninstall com.gathin.tvhome`. It's an
     ordinary app.
- macOS: if `adb connect` fails with "No route to host" even though the TV is
  reachable, use the repo's `tools/tv.py` Python client as its README
  describes.

## Getting adb
Check whether `adb` is already on PATH. If not, don't install anything
system-wide:
- Windows: download
  `https://dl.google.com/android/repository/platform-tools-latest-windows.zip`,
  extract it to `google-tv/platform-tools/`, and run
  `google-tv\platform-tools\adb.exe`.
- macOS: the same, with `platform-tools-latest-darwin.zip`.
- Linux: the same, with `platform-tools-latest-linux.zip`.

## Connecting
The owner turns on developer options and USB debugging (guide step 2) and
gives you the TV's IP address.
1. `adb connect <ip>:5555`. The TV shows "Allow USB debugging?". The owner
   ticks "Always allow from this computer" and presses OK. Then run
   `adb devices`; it must show `<ip>:5555  device`. If it shows
   `unauthorized`, the owner hasn't accepted the prompt yet.
2. If the connection is refused, the TV probably uses **Wireless debugging**
   (Developer options → Wireless debugging → Pair device with pairing code).
   Run `adb pair <ip>:<pairing-port>` with the 6-digit code, then
   `adb connect <ip>:<port shown on the Wireless debugging screen>`.
3. The PC must be on the same network as the TV. Guest Wi-Fi or a VPN will
   block this.

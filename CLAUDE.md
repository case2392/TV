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

# TCL Google TV: what's safe to switch off

Every change here is a **disable** (`pm disable-user --user 0`), not a delete,
so `pm enable <package>` puts it back. A factory reset also restores
everything.

Package names vary between TCL models and regions, so only act on packages
that are actually installed. The descriptions come from community debloat
lists that were tested on TCL Google TVs. Where a package's purpose isn't well
documented, the table says so.

## Tier 1: Recommended (ads, promos and TCL extras)
Confirm the list once, then disable them as a batch.

| Package | What it is |
|---|---|
| `com.tcl.bootadservice` | TCL ad service (ads inside TCL apps and at boot) |
| `com.tcl.partnercustomizer` | TCL partner/promo customisations |
| `com.tcl.exhibit` | TCL showcase/demo content |
| `com.tcl.ocean.instructions` | TCL on-screen instructions |
| `com.tcl.esticker` | TCL sticker/overlay service |
| `com.tcl.copydatatotv` | TCL data-transfer tool |
| `com.tcl.notereminder` | TCL notes & reminders |
| `com.tcl.gallery` | TCL photo gallery |
| `com.tcl.magiconnectfree` | TCL Magiconnect (phone companion) |
| `com.tcl.usercenter` | TCL account centre |
| `com.tcl.smartalexa` | Alexa integration. **Keep it if you control the TV with Alexa** |
| `com.google.android.play.games` | Google Play Games |
| `com.google.android.feedback` | Google feedback reporter |

## Tier 2: Optional (ask about each one; people do use these)

| Package | What it is / what you lose |
|---|---|
| Pre-installed streaming apps (Netflix, Prime Video, Tubi, Pluto TV, etc.) | Find their package names with `pm list packages`. Remove only the ones you never use |
| `com.google.android.youtube.tvmusic` | YouTube Music |
| `com.google.android.videos` | Google TV / Play Movies purchases |
| `com.google.android.apps.mediashell` | **Chromecast built-in.** Disabling it stops casting from your phone |
| `com.tcl.ui_mediaCenter` | USB media player (playing files from a USB stick) |
| `com.tcl.MultiScreenInteraction_TV` | TCL phone-to-TV screen sharing |
| `com.tcl.miracast` | Miracast screen mirroring |
| `com.tcl.assistant` | TCL's own voice assistant (not Google Assistant) |
| `com.google.android.tvrecommendations` | Home-screen recommendations. Only matters if you keep the stock home screen |

## Tier 3: Aggressive (one at a time, then test)
Community lists disable these, but what they do isn't well documented. After
each one, test the Home button, the Inputs/HDMI switching, the remote's
buttons, and voice search before doing the next.

| Package | Notes |
|---|---|
| `com.tcl.dashboard` | TCL dashboard. **Check the remote's settings/quick-menu button still works** |
| `com.tcl.messagebox` | TCL message box / notices |
| `com.tcl.overseasappshow` | TCL app promotions |
| `com.tcl.bi` | Likely TCL analytics (undocumented) |
| `com.tcl.t_solo` | Undocumented TCL service |
| `com.tcl.tv.tclhome_passive` | Undocumented TCL home service |
| `com.tcl.useragreement` | TCL user-agreement prompts |
| `com.tcl.appmarket2` | TCL app store |
| `com.tcl.guard` | TCL "Safety Guard" (cleaner / permission manager) |

## Never touch
These have caused boot loops or broken basic functions in community reports,
or the TV needs them. **Don't disable them, even if asked to "remove
everything".**

| Package | Why |
|---|---|
| `com.tcl.waterfall.overseas` | Reported stuck-at-Google-TV-logo boot loop |
| `com.android.location.fused` | Reported boot loop |
| `com.google.android.apps.tv.launcherx` | The stock home screen. Only change it through the launcher-swap step, after a replacement is installed. Otherwise you get a blank screen |
| `com.google.android.tungsten.setupwraith` | Setup wizard. Only change it through the launcher-swap step |
| `com.tcl.suspension` | Reported to kill the remote's Inputs button |
| `com.tcl.tv`, `com.tcl.tvinput`, `com.tvos`, `com.tcl.system.server` | Core TCL TV system: tuner, inputs, picture |
| `com.tcl.autopair`, `com.tcl.tcl_bt_rcu_service` | Remote control pairing / Bluetooth remote |
| `com.tcl.versionUpdateApp`, `com.tcl.rc.ota` | Firmware/security updates |
| `com.tcl.initsetup` | First-run setup |
| `com.android.tv.settings` | The Settings app |
| `com.android.vending`, `com.google.android.gms`, `com.google.android.gsf` | Play Store and Google Play Services |
| `com.google.android.katniss` | Google voice search / Assistant |
| `com.google.android.webview` | Many apps need it to show web content |
| anything starting `com.android.providers.` | Core Android data stores |

Sources: [TCL 65C7K list](https://gist.github.com/llaumgui/e39fdbafc920edacf42fdff6d8ab3a0a),
[TCL debloat gist and comments](https://gist.github.com/0x4f53/27bee9a0069f7bd6a2aa9eb60bb10d1e),
[audited TCL table](https://github.com/chrissy-dev/debloat-tlc-android-tv).

# Clean Your TV launcher (personal fork)

This is a personal fork of github.com/Gathin23/cleanyourtv, used to build a custom home screen for Android TV and Google TV. The remote `origin` is the fork (DonaldGonzalez/cleanyourtv) and `upstream` is the original. The repo is public, so nothing personal goes into committed files: no IP addresses, Wi-Fi names, serial numbers or screenshots. Device details live in CLAUDE.local.md, which git ignores.

## Layout

- `launcher/`: the launcher app (Java, Gradle, package `com.gathin.tvhome`). Source is in `launcher/app/src/main/java/com/gathin/tvhome/`, built-in settings in `config.xml`.
- `tools/`: Python and shell helpers. `tv.py` is an adb-over-TCP client (needs `TV_HOST`, port 5555 by default). `theme.sh` pushes a theme. `disable.sh` and `restore.sh` apply and undo debloat lists.
- `themes/`: `config.json` overlays that change dock, columns, colors and wallpaper without a rebuild. Keys are documented in `themes/README.md`.
- `docs/`: upstream notes. `docs/watch-next.md` covers reading the Continue Watching (Watch Next) rows.
- `examples/`: per-model debloat lists from contributors.

## Build, install, check

Java 17 and the Android SDK are already installed on the Mac. The Python tools need `tools/venv`.

```bash
TV_HOST=<tv ip> launcher/build-and-install.sh
```

```bash
TV_HOST=<tv ip> tools/theme.sh themes/<name>
```

```bash
TV_HOST=<tv ip> tools/venv/bin/python -I tools/tv.py --screenshot shots/<name>.png
```

`shots/`, `work/` and `backup/` are gitignored. Screenshots and working notes go there, never in a commit.

## How to work in this repo

- If a change can be made with a theme `config.json`, do it as a theme. Only change Java code when a theme can't do it.
- One branch per feature. Show the plan before editing code.
- Before installing a new build, copy the APK currently on the TV (the last one that worked) to `~/tv-builds/launcher-known-good.apk`. A broken launcher leaves the TV with no home screen, because the stock launcher is disabled.
- After each install, take a screenshot, look at it, then ask Donald to check with the remote: Home button, app launch, back from an app, voice search.
- Never disable, uninstall or enable a package on the TV without asking first.
- Don't change `applicationId` or the package name. Upstream scripts look for `com.gathin.tvhome`.
- Keep changes easy to merge with upstream: prefer new classes and files over large edits to upstream files, and say in the commit message which upstream files changed.

## Pulling upstream changes

```bash
git fetch upstream
git merge upstream/main
```

Build and test on the TV after every merge. Upstream moves fast.

## Recovery

```bash
adb install -r ~/tv-builds/launcher-known-good.apk
```

If no good build exists, bring back Google's home screen:

```bash
adb shell pm enable com.google.android.apps.tv.launcherx
```

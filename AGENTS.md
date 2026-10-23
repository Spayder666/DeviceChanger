# AGENTS.md

## Cursor Cloud specific instructions

### What this repository is

This is **not** a buildable source-code project. It is a static **distribution / asset
repository** for the "Device Changer" (DC) Android app. It contains only:

- `Device_Changer_v.*.apk` — prebuilt Android app binaries (the released app itself).
- `DeviceProfile/*.prop` — device property profiles consumed by the app to spoof/change
  device fingerprints (plain `key=value` text, `#` comments, blank-line separated).
- `update.json`, `Update.json`, `Update_DT.json` — in-app updater manifests
  (`versionCode`, `versionName`, `downloadUrl`, `releaseNotes`). The app reads these and
  downloads the APK at `downloadUrl`.

There is **no** source code, package manager, lockfile, build system, or automated test
suite. Do not look for `package.json`, Gradle, a Makefile, etc. — there are none. There is
nothing to compile and no dev server to run; the "application" is the prebuilt APK, whose
source is not in this repo.

### Tooling

`jq`, `python3`, `node`, `java`, `unzip`, and `curl` are preinstalled and are all that is
needed to inspect/validate the assets. No dependency install step is required (the update
script is a no-op).

### How to validate the assets ("the environment works" check)

There is no app to launch from source. Validate the artifacts the repo actually serves:

- JSON manifests are well-formed and have required keys:
  `jq -e '.versionCode, .versionName, .downloadUrl, .releaseNotes' update.json`
- Device profiles parse: each `DeviceProfile/*.prop` is `key=value` lines (ignore blank/`#`).
- APK integrity: an APK is a ZIP; confirm `AndroidManifest.xml` + `classes.dex` are present
  via `unzip -l <file.apk>`.
- Updater flow: `curl -sIL "$(jq -r .downloadUrl update.json)"` should return a downloadable
  Android package (`content-type: application/vnd.android.package-archive`).

### Notes / gotchas

- `update.json` (lowercase) points to the production site (`deviceschanger.org`, currently
  versionName `3.263`); `Update.json` / `Update_DT.json` point to GitHub raw of the in-repo
  `3.104` APK. They are intentionally different manifests, not duplicates.
- `Update_DT.json`'s `downloadUrl` contains a Cyrillic "С" in `..._DС.apk` (differs from the
  ASCII filenames in the repo). Preserve byte-for-byte unless explicitly asked to fix.
- Fully running the app would require an Android device/emulator plus root/Magisk (the app
  modifies system properties); that is out of scope for this repo since no source is present.

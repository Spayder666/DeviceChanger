# AGENTS.md

## Cursor Cloud specific instructions

### What this repository is

This is **not a buildable software project**. It is a static **asset/distribution repository**
for the "Device Changer" (Device Changer DC) Android app. It contains only release artifacts
and data files:

- `Device_Changer_v.*.apk` — prebuilt Android APK release binaries (the app's source is **not**
  in this repo).
- `DeviceProfile/*.prop` — Android device property profiles (build-fingerprint / `ro.*` key=value
  spoofing profiles for many phones/tablets).
- `update.json`, `Update.json`, `Update_DT.json` — in-app updater manifests
  (`versionCode`, `versionName`, `downloadUrl`, `releaseNotes`).

### Setup / build / run

- There is **nothing to install, build, or serve**. There is no language runtime, package
  manager, dependency manifest, service, or test framework. The startup update script is
  intentionally a no-op.
- Do **not** add a dependency-install step or attempt to start a dev server — none applies.

### Core functionality and how to validate it

The app's core mechanism represented here is the **in-app updater**: the app reads an update
manifest (JSON) and downloads the APK at its `downloadUrl`. You can validate the assets
end-to-end with only base tools (`jq`, `unzip`, `file`, `curl`):

- Validate manifests: `jq -e 'has("versionCode") and has("versionName") and has("downloadUrl")' update.json`
- Validate an APK is a real Android package: `file Device_Changer_v.3.104_DC.apk` and
  `unzip -l Device_Changer_v.3.104_DC.apk | grep -E 'AndroidManifest.xml|classes.dex'`
- Validate profiles parse as key=value: non-empty, non-`#`, non-separator (`---`) lines contain `=`.
- Simulate the updater: read a manifest's `downloadUrl` and `curl -I -L` it. `Update.json` points
  at this repo's GitHub raw `v.3.104` APK (content-length matches the local file);
  `update.json` points at the live `deviceschanger.org` host.

Note: `Update_DT.json`'s `downloadUrl` intentionally contains a Cyrillic `С` in the filename;
this is data, not a typo to "fix".

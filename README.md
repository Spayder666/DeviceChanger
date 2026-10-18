<div align="center">

# Devices Changer

**Spoof identity, build, network, location, and environment on a rooted Android device**

Privacy · app testing · API checks against another phone

<br>

[![Version](https://img.shields.io/badge/version-4.400-blue?style=for-the-badge)](https://deviceschanger.org)
[![Android](https://img.shields.io/badge/Android-11%2B-3DDC84?style=for-the-badge&logo=android&logoColor=white)](https://developer.android.com)
[![Root](https://img.shields.io/badge/Root-required-orange?style=for-the-badge)]()
[![Zygisk](https://img.shields.io/badge/Zygisk-required-2196F3?style=for-the-badge)]()

<br>

[![Website](https://img.shields.io/badge/website-deviceschanger.org-0A7?style=flat-square)](https://deviceschanger.org)
[![Telegram](https://img.shields.io/badge/Telegram-@DeviceChanger-26A5E4?style=flat-square&logo=telegram&logoColor=white)](https://t.me/DeviceChanger)
[![License bot](https://img.shields.io/badge/bot-@DeviceChanger__bot-26A5E4?style=flat-square&logo=telegram&logoColor=white)](https://t.me/DeviceChanger_bot)

</div>

---

## Screenshots

<table>
  <tr>
    <td align="center"><a href="screenshots/home.png"><img src="screenshots/thumbs/home.png" alt="Home" width="160"><br><b>Home</b></a></td>
    <td align="center"><a href="screenshots/targets.png"><img src="screenshots/thumbs/targets.png" alt="Targets" width="160"><br><b>Targets</b></a></td>
    <td align="center"><a href="screenshots/hooks.png"><img src="screenshots/thumbs/hooks.png" alt="Hooks" width="160"><br><b>Hooks</b></a></td>
    <td align="center"><a href="screenshots/location.png"><img src="screenshots/thumbs/location.png" alt="Location" width="160"><br><b>Location</b></a></td>
  </tr>
</table>

## What it is

**Devices Changer** (`com.motorola.backup`) is a control panel for a rooted phone. It presents a chosen device, network, and location to apps and to the system, so you can test software or keep a consistent identity.

It needs **Android 11+**, **root** (Magisk or KernelSU), and **Zygisk**. An access code from the [bot](https://t.me/DeviceChanger_bot) or the [site](https://deviceschanger.org) unlocks the tools. Current build: **4.400** (1770).

## What it can do

- **Identifiers** — Android ID, GSF ID, IMEI, Wi-Fi and Bluetooth addresses, advertising ID, Media DRM, SIM data
- **Device profile** — model, brand, fingerprint, kernel, user agent, and the rest of the build
- **SIM and cell** — operator, signal, slots, synthetic towers
- **Wi-Fi** — saved networks, scan results, the current network
- **Location** — coordinates, satellites, and related sensors
- **Per app** — a separate hook set and IDs for selected packages
- **Hide** — hide installed apps from chosen targets
- **VPN** — hide a VPN from apps, or run your own tunnel
- **Camera** — replace the camera picture with a file or a stream
- **Browser** — a fingerprint profile for the companion browser
- **Keybox** — attestation keybox, with TEESimulator and Play Integrity Fix
- **Cleanup and backup** — local cleanup, and a cloud backup of profiles and settings

A home-screen widget shows the profile, modules, root, license, and network status.

---

Use it on your own device, for privacy and testing. You are responsible for how you use it.

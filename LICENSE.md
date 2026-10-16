# BioBridge v1.5 — Universal Biometric Middleware (Made in Egypt)

BioBridge brings an old laptop fingerprint reader back to life on Windows 10/11, after Windows itself stopped supporting it.
It reads the sensor directly, matches your fingerprint, and puts a full-screen lock in front of you as soon as Windows starts.

## ⚠️ Supported hardware — please read first
**Supported: UPEK / AuthenTec TouchChip Fingerprint Coprocessor, USB ID `147E:2020` only**
(found in Lenovo ThinkPads of the W530 / T430 / T530 / X230 generation).

Check before you install anything:
1. Open **Device Manager** → your fingerprint reader → **Properties** → **Details** → **Hardware Ids**.
2. It must show **`VID_147E&PID_2020`**. Any other ID is **not supported** — please don't try.

**If your fingerprint reader already works with Windows Hello, do NOT use BioBridge.**
Switching the driver to WinUSB (step 1 below) disables Windows Hello for that reader.

Tested on: ThinkPad W530, Windows 10.

## Features
- **Full-screen lock** at sign-in: fingerprint or a 5-character PIN, no other way past it
  (Windows key, Alt+Tab, Alt+F4, Task Manager shortcut are blocked).
- **Re-locks** when the PC wakes from sleep or comes back from Win+L.
- **Full-lock mode**: Windows signs in by itself and BioBridge is the first screen you see
  (installs to `C:\Program Files\BioBridge`, works on battery and AC).
- **Encrypted vault** for files and notes (AES-256 + HMAC, key protected by Windows DPAPI).
- **Key backup** to the **TPM chip** (1.2 or 2.0) and/or a USB flash drive, protected by your PIN.
- **Local API** (`127.0.0.1:47321`) so apps and websites on the same PC can ask for a fingerprint confirmation,
  with signed (ECDSA P-256) challenge responses.
- Arabic and English interface.

## Installation
1. **Zadig** (https://zadig.akeo.ie): Options → List All Devices → choose **TouchChip Fingerprint Coprocessor**
   (USB ID `147E 2020`) → select **WinUSB** → Replace Driver.
   ⚠️ Make sure you select the fingerprint reader — changing the driver of another device (keyboard, touchpad,
   Bluetooth, USB hub) will stop that device from working.
2. Unzip the release, double-click **`build.bat`** (uses the .NET Framework 4 compiler built into Windows).
   It creates `BioBridge.exe`. If the build fails, see `build_log.txt`.
3. Run `BioBridge.exe`, create your 5-character PIN, then enroll your finger (4 swipes).
4. Optional: **Settings → Full-lock mode → Enable** (asks for your Windows password and admin rights once).

## Good to know
- **Match threshold**: it was tuned on one finger. If you get too many rejections or easy matches,
  adjust it in Settings. Scores are written to `%APPDATA%\TouchChipFP\log.txt`.
- **TPM backup** needs the security chip enabled in the BIOS (Security → Security Chip → Active).
- **Microsoft account sign-in** (email): automatic sign-in may need extra setup with Windows' own `netplwiz`.
- **Soft lock**: BioBridge locks the screen on top of Windows. Ctrl+Alt+Del and Safe Mode are protected by Windows
  and cannot be blocked by any program. It is not a replacement for a strong Windows password.

## Undo / uninstall
- Settings → Full-lock mode → **Disable** (removes automatic sign-in and the startup task).
- To give the reader back to Windows: Device Manager → fingerprint reader → Uninstall device →
  tick "Delete the driver software" → restart.

## License & credits
LGPL-2.1-or-later. The sensor protocol is ported to C# from libfprint's `upektc_img` driver
(Copyright (C) 2013 Vasily Khoruzhick).

Made in Egypt 🇪🇬 by Ahmed Elmhor.

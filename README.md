# GATA controller firmware — signed release channel

This repository is the **download source** for the GATA Cloud Uploader.
Installed copies of the updater read `manifest.json` from here and verify it
against `manifest.json.sig` using the public key pinned inside the app.

* `manifest.json`      — the version list (newest first) with SHA-256 per file
* `manifest.json.sig`  — ECDSA P-256 signature of manifest.json (**required**)
* `M_*.bin`            — controller application (external flash @0x90000000)
* `B1.bin` / `B3.bin`  — resident system firmware (internal flash @0x08000000)
* `esp/*.bin`          — GATE cloud-module (ESP32) images

**Do not edit files here by hand.** Publish with the release helper in the
app repository (`Software/gata-updater`):

```powershell
tools\publish_firmware.ps1 -Version 16.8.27 -Main C:\build\M_16_8_27.bin -Notes "..." -Push
```

It validates the images, computes hashes, signs the manifest with the offline
private key and pushes here. A manifest without a valid signature is refused
by every installed updater, so a tampered file on this server cannot reach a
controller.

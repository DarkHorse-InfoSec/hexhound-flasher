# HexHound Web Flasher

Flash [HexHound](https://github.com/DarkHorse-InfoSec) onto an ESP32 board from
your browser. Pick your board, plug it in over USB, hit install.

**Flasher: https://darkhorse-infosec.github.io/hexhound-flasher/**

HexHound is a cyber recon pet: a virtual creature that lives on a small ESP32
handheld, evolves as you carry it around, and reacts to the wireless world it
sees. Built by [DarkHorse Information Security](https://darkhorseinfosec.com).

This repository holds only the flasher page and the prebuilt firmware images.
The firmware source lives in a separate repository.

<!-- BOARDS:BEGIN -->
## Firmware v0.4.3

| Board | Chip | Size | SHA256 (first 16) |
|---|---|---|---|
| LilyGo T-Dongle S3 | `ESP32-S3` | 1259 KB | `795224bc7bc2006a` |
| LilyGo T-Dongle S3 (USB HID) | `ESP32-S3` | 1309 KB | `797e52a025dbcc72` |
| LilyGo T-Display S3 | `ESP32-S3` | 1369 KB | `9bb1c2e0131daaa6` |
| Waveshare 1.47B | `ESP32-S3` | 1396 KB | `883a27bb0925beb0` |
| Waveshare Touch 1.47 | `ESP32-S3` | 1409 KB | `31fb034742bdb29d` |
| LilyGo T-Dongle C5 | `ESP32-C5` | 1911 KB | `7b6e234b4f41fac5` |
| Waveshare 1.28 Round | `ESP32-S3` | 1593 KB | `13449e17ebd2ff6b` |
<!-- BOARDS:END -->

Full hashes are in [`boards.json`](boards.json). These are merged images that
flash at offset `0x0`.

## Before you flash

**Pick the environment that matches your board.** Every ESP32-S3 image reports
the same chip family, so nothing stops you flashing the wrong one. The result is
working firmware driving the wrong display pins, which looks exactly like a dead
board. If your screen stays dark after a successful flash, this is almost
certainly why.

**Flashing wipes the pet.** These are full flash writes, so an existing pet and
its save are erased. Fine on a new board; less fine on one you have been
carrying.

**Chrome or Edge on desktop only.** Flashing uses
[Web Serial](https://developer.mozilla.org/en-US/docs/Web/API/Web_Serial_API),
which does not exist in Firefox or Safari, or on mobile. The page detects this
and points you at the esptool route below.

**ESP32-C5 support in the browser is unverified.** The C5 manifest declares
`chipFamily: "ESP32-C5"`. If the pinned ESP Web Tools release does not know that
chip yet, the button will error. That is the web tooling, not the image: the C5
binary flashes fine with `esptool`.

## Flashing with esptool instead

Works on every platform and for every board, including the C5.

```bash
pip install esptool

# ESP32-S3 boards
esptool --chip esp32s3 --port <PORT> --baud 921600 write-flash 0x0 firmware/<IMAGE>.bin

# T-Dongle C5
esptool --chip esp32c5 --port <PORT> --baud 921600 write-flash 0x0 firmware/hexhound-t-dongle-c5.bin
```

After flashing, **power-cycle the board** by unplugging and replugging USB. An
esptool reset alone can leave it sitting in download mode with a dark screen.

## First boot

The pet starts as an Egg. A **short press** starts a patrol, a **long press**
opens the menu. It evolves as it accumulates experience from what it sees.

## Updating later

v0.4.2 is the first release that can be updated over the air, so images flashed
from this page can take future updates without USB. Anything flashed before this
release has to be re-flashed over USB once to gain that.

## License

[MIT](LICENSE). Copyright (c) 2026 DarkHorse Information Security.

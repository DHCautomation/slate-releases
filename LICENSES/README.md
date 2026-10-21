# Third-party licenses of the SlateTap releases

**This is not a legal reading.** The folder collects the license texts and notices of the third-party software in
the binaries published on this site, as far as the release tooling can find them in the build's tree. Whether it
meets every obligation of every license is for DHC Automation and Controls to decide.

## What each image holds

| Image | Third-party software | Here |
|---|---|---|
| `slatetap-<version>.bin` (firmware) | ESP-IDF and the components it brings (FreeRTOS, lwIP, newlib, wpa_supplicant, http_parser, Mbed TLS, ESP-MQTT, cJSON), Espressif's mDNS component, esp_littlefs with LittleFS, Nayuki's QR Code generator, bacnet-stack, the X11 misc-fixed bitmap fonts | `COMPONENTS.txt` (versions, licenses, sources) and one folder per component |
| `www-<version>.bin` (web app) | the web app's npm packages (React, React Flow and its d3 modules, the Inter, Outfit and JetBrains Mono fonts) | `web-app-packages.txt` |
| `slatetap-<version>-merged.bin` (USB image) | the two above, plus ESP-IDF's second-stage bootloader | as above |

## Folders

| Folder | License |
|---|---|
| `esp-idf/` | Apache-2.0 (`LICENSE`); `COPYRIGHT.rst` is ESP-IDF's own list of the third-party code it includes |
| `mbedtls/`, `esp-mqtt/`, `esp-mdns/` | Apache-2.0 |
| `freertos/`, `cjson/`, `qrcodegen/`, `esp_littlefs/`, `http_parser/` | MIT |
| `lwip/`, `littlefs/`, `wpa_supplicant/` | BSD-3-Clause |
| `newlib/` | BSD and others, per file (`COPYING.NEWLIB`) |
| `bacnet-stack/` | GPL-2.0-or-later with the GCC linking exception, MIT and BSD-3-Clause, per file (`FILES.txt`); see below |

## bacnet-stack and our patch

The core files of bacnet-stack are licensed GPL-2.0-or-later WITH GCC-exception-2.0: the firmware may link them
without becoming GPL, while changes to the stack's own files must be made available when the binaries are
distributed. SlateTap compiles the files listed in `bacnet-stack/FILES.txt` from the upstream tag named there,
unmodified except `src/bacnet/datalink/bsc/bvlc-sc.c`, to which `bacnet-stack/bvlc-sc-byte-order.patch` is applied
at build time (BVLC-SC 2-octet fields sent most significant octet first, as Annex AB specifies). The upstream source
of that tag is at https://github.com/bacnet-stack/bacnet-stack; with the patch it is the source of every bacnet-stack
file in the firmware. `bacnet-stack/license-readme.txt` is upstream's license note; `GPL-2.0`, `GCC-exception-2.0`, `MIT`
and `BSD-3-CLAUSE` are its license texts.

## How this folder is made

`README.md` and `bacnet-stack/README.txt` are written by hand. Everything else is refreshed from the release build's
tree each time a release is published: the license files are copied as the components ship them, and
`COMPONENTS.txt`, `bacnet-stack/FILES.txt` and `web-app-packages.txt` are generated (the last from the web app's
production dependencies as installed for that build).

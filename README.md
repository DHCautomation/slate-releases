# slate-releases

Firmware releases of DHC Automation and Controls' Slate products (SlateTap ST-1), served by GitHub Pages at
https://dhcautomation.github.io/slate-releases/ (later also https://releases.dhcautomation.ca/). A SlateTap checks its
channel's `versions.json` for updates; a technician flashes a new or wiped unit from a version's page in Chrome or
Edge (Web Serial needs the HTTPS these pages have).

**Do not edit this repository by hand.** Every change is made by `tools/release.ps1 -Publish` in the firmware
repository, which checks what it writes before it commits and pushes.

## Layout

```
index.html                                 every release per product and channel, with links to the flasher pages
slatetap-st1/stable/versions.json          the update feed of a channel (manifest format 2, compact JSON, 15 KB at most)
slatetap-st1/stable/notes.json             the full notes of the versions the feed lists
slatetap-st1/beta/...                      betas, after 24 h on the bench; every stable release is listed here too
slatetap-st1/test/...                      the bench unit's channel (an override URL), never offered by a unit itself
files/<version>/                           one release: slatetap-<version>.bin (firmware), www-<version>.bin (web app),
                                           slatetap-<version>-merged.bin (USB image from offset 0), manifest.json and
                                           index.html (the flasher), notes.txt, SHA256SUMS.txt
LICENSES/                                  third-party licenses and the bacnet-stack patch (LICENSES/README.md)
.nojekyll                                  files are served as they are
.gitattributes                             no line-ending conversion in any clone (the hashes of a version must hold)
CNAME                                      the custom domain, once set in Settings > Pages (GitHub writes it)
```

## Rules the release tooling keeps

- A version folder is written once and never changed: units that downloaded an image, and the CDN, rely on its
  hash. A fix is a new version.
- A channel's feed lists its 10 newest versions plus two anchors, 0.2.0 (the field release) and the newest stable
  release, so a unit can always download the web app of the firmware it rolls back to.
- A version reaches `beta` only after it has been published to `test`, pulled by the bench unit from this host and
  run for 24 h; promotion lists the same files again, it never rebuilds them.
- `files/` keeps the versions a feed lists and every stable release; git history keeps the rest.
- Nothing here is secret or private: no source code, build logs or debug files, no credentials or keys, no unit
  backups or diagnostics, no customer or site names. Release notes name features and fixes only.

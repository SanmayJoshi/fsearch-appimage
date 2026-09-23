# FSearch AppImage (Automated Builds)

[![Build Script](https://img.shields.io/badge/build-script-blue)](https://github.com/SanmayJoshi/fsearch-appimage/actions/workflows/build.yml)
[![Continuous Build](https://img.shields.io/github/v/release/SanmayJoshi/fsearch-appimage?include_prereleases&label=continuous%20build&color=blue)](https://github.com/SanmayJoshi/fsearch-appimage/releases/tag/continuous)
[![GitHub Release (Latest by date)](https://img.shields.io/github/v/release/SanmayJoshi/fsearch-appimage?label=Latest%20Stable)](https://github.com/SanmayJoshi/fsearch-appimage/releases/latest)
[![Upstream Repository](https://img.shields.io/badge/Upstream-cboxdoerfer%2Ffsearch-informational)](https://github.com/cboxdoerfer/fsearch)
[![License: GPL-2.0-or-later](https://img.shields.io/badge/License-GPL%202.0%2B-green.svg)](https://github.com/cboxdoerfer/fsearch/blob/master/COPYING)

Automated, dependency-bundled AppImage packaging for **[FSearch](https://github.com/cboxdoerfer/fsearch)**, the fast file search utility for Linux.

This repository tracks upstream releases and development branches, packaging them into standalone, portable Linux executables featuring embedded delta update support.

---

## Release Channels

Builds are monitored and released under two distinct tracks:

| Channel | Source Reference | Release Type | Target Audience | Delta Update Track |
| :--- | :--- | :--- | :--- | :--- |
| **Stable** | Official upstream git tags (`releases/latest`) | **Full Release** | General daily use; maximum stability | Tracks official releases |
| **Continuous** | Latest commit on upstream `master` | **Pre-release** | Testers; access to cutting-edge features | Tracks daily development builds |

### Direct Downloads

- **[Latest Stable Release](https://github.com/SanmayJoshi/fsearch-appimage/releases/latest)**: Best for most users.
- **[Continuous Build](https://github.com/SanmayJoshi/fsearch-appimage/releases/tag/continuous)**: Automatically refreshed when upstream commits land on `master`.

---

## Installation and Execution

AppImages run out-of-the-box without root permissions or system installation.

### 1. Download
Download the `.AppImage` binary from the [Releases](https://github.com/SanmayJoshi/fsearch-appimage/releases) page.

### 2. Mark Executable
Open your terminal in the download folder and run:
```bash
chmod +x FSearch-*.AppImage
```
*(Or right-click the file in your desktop file manager -> Properties -> Permissions -> Allow executing file as program).*

### 3. Run
```bash
./FSearch-*.AppImage
```
*Or simply double click the file.*

> [!NOTE]
> **FUSE Requirement (Ubuntu 22.04+, Debian 12+, Fedora):**  
> Modern Linux distributions may require `libfuse2` to mount AppImages. If the file does not launch, install it via:
> ```bash
> # Debian / Ubuntu / Mint
> sudo apt install libfuse2
>
> # Fedora / RHEL
> sudo dnf install fuse-libs
>
> # Arch Linux / Manjaro
> sudo pacman -S fuse2
> ```

---

## Delta Updates (Zsync)

Every release includes an embedded update signature pointing to a `.zsync` control file. This allows updating to newer releases by downloading **only changed byte-ranges** (often just 2–5 MB) rather than re-downloading the entire AppImage.

### Using AppImageUpdate
You can update in-place using the official CLI or GUI tool:

```bash
# Update an existing binary in-place
appimageupdate ./FSearch-x86_64.AppImage
```

- If you downloaded a **Stable** release, your updater tracks the `latest` stable channel.
- If you downloaded a **Continuous** release, your updater tracks the continuous `master` channel.

Alternatively, integration managers like [AppImageLauncher](https://github.com/TheAssassin/AppImageLauncher) or [Gear Lever](https://github.com/mijorus/gearlever) can detect and apply these updates automatically.

---

## Checksum Verification

Every release includes matching `.sha256` files to verify binary integrity.

To verify a downloaded AppImage:
```bash
sha256sum -c FSearch-x86_64.AppImage.sha256
```
Expected output:
```text
FSearch-x86_64.AppImage: OK
```

---

## How the Automation Works

The workflow runs on a daily schedule (`00:00 UTC`) via GitHub Actions:

```text
[Cron: 00:00 UTC]
        │
        ├─► Check Upstream Releases ──► New tag found? ──► Build Tag & publish as Stable Release
        │
        └─► Check Upstream Master   ──► New SHA found? ──► Build AppDir & update Continuous Pre-release
```

1. **Upstream Tag Detection:** Checks the GitHub API for the latest official tag on `cboxdoerfer/fsearch`. If there isn't a published release for that tag, a build is queued.
2. **Commit Tracking:** Compares the upstream `master` branch head SHA with the SHA recorded in the current `continuous` release body. If upstream has moved ahead, a new continuous build is queued.
3. **Packaging:** Bundles FSearch with GTK3 dependencies using `linuxdeploy` and `linuxdeploy-plugin-gtk`.
4. **Housekeeping:** Previous commit-specific binaries in the `continuous` release tag are automatically purged to prevent indefinite artifact storage accumulation.

---

## LLM Disclaimer

The CI/CD workflow automation, packaging recipes, and repository documentation were drafted and maintained with the assistance of Large Language Models (LLMs).

- **Core Application:** The underlying application source code is authored and maintained exclusively by Christian Boxdörfer and upstream contributors.
- **As-Is Basis:** While the pipeline is reviewed to follow AppImage and Linux packaging best practices, the scripts and configurations are provided "as is", without warranty of any kind.

---

## Disclaimer & Credits

- **FSearch** is developed and maintained by **[Christian Boxdörfer (cboxdoerfer)](https://github.com/cboxdoerfer)** and contributors under the [GNU General Public License v2.0](https://github.com/cboxdoerfer/fsearch/blob/master/COPYING).
- All code rights, icons, and trademarks belong to the [FSearch](https://github.com/cboxdoerfer/fsearch) project under the GPL-2.0-or-later license.
- This repository is an **independent, automated build service** providing community AppImage packaging. It is not officially affiliated with or endorsed by upstream.
- For issues regarding FSearch itself, report them at [cboxdoerfer/fsearch/issues](https://github.com/cboxdoerfer/fsearch/issues).
- For packaging-related problems (e.g., missing runtime libraries, update failures, workflow bugs), open an issue in this repository.
- **No Warranty:** The AppImages and automated packaging scripts in this repository are provided on an **"AS IS" basis, without warranty of any kind**, express or implied. Use them at your own risk. The maintainer assumes no liability for any damages, data loss, or system issues resulting from downloading, executing, or updating these binaries.

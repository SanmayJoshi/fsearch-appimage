# FSearch Continuous AppImage

[![Build and Release AppImage](https://github.com/SanmayJoshi/fsearch-appimage/actions/workflows/build.yml/badge.svg)](https://github.com/SanmayJoshi/fsearch-appimage/actions/workflows/build.yml)
[![Continuous Release](https://img.shields.io/github/v/release/SanmayJoshi/fsearch-appimage?include_prereleases&label=continuous%20build&color=blue)](https://github.com/SanmayJoshi/fsearch-appimage/releases/tag/continuous)
[![License: GPL-2.0-or-later](https://img.shields.io/badge/License-GPL%202.0%2B-green.svg)](https://github.com/cboxdoerfer/fsearch/blob/master/COPYING)

This repository provides **automated continuous AppImage builds** tracking the latest upstream [`master`](https://github.com/cboxdoerfer/fsearch) branch of [**FSearch**](https://github.com/cboxdoerfer/fsearch), a fast file search utility for Linux inspired by Everything Search Engine.

---

## Features

- **Automated Upstream Sync:** A scheduled GitHub Actions workflow checks upstream daily. If new commits are detected, a fresh AppImage is built and published automatically.
- **Static Direct Download Link:** Provides an unversioned `FSearch-x86_64.AppImage` link that always points to the latest continuous build.
- **Delta-Update Ready:** Bundled with embedded `.zsync` metadata for fast, bandwidth-friendly differential updates via [AppImageUpdate](https://github.com/AppImageCommunity/AppImageUpdate).
- **Integrity Verified:** Every release includes SHA-256 checksum files for verification.

---

## Download

### Latest Build (Static Links)

| Artifact | Download Link | Description |
| :--- | :--- | :--- |
| **AppImage** | [FSearch-x86_64.AppImage](https://github.com/SanmayJoshi/fsearch-appimage/releases/download/continuous/FSearch-x86_64.AppImage) | Always points to latest master build |
| **Checksum** | [FSearch-x86_64.AppImage.sha256](https://github.com/SanmayJoshi/fsearch-appimage/releases/download/continuous/FSearch-x86_64.AppImage.sha256) | SHA-256 verification hash |
| **Zsync File** | [FSearch-x86_64.AppImage.zsync](https://github.com/SanmayJoshi/fsearch-appimage/releases/download/continuous/FSearch-x86_64.AppImage.zsync) | Metadata for differential updates |

*(Commit-specific builds e.g. `FSearch-<short_sha>-x86_64.AppImage` are also available on the [Continuous Releases](https://github.com/SanmayJoshi/fsearch-appimage/releases/tag/continuous) page).*

---

## Quick Start

### 1. Download and Run

```bash
# Download latest AppImage
wget https://github.com/SanmayJoshi/fsearch-appimage/releases/download/continuous/FSearch-x86_64.AppImage

# Make executable and launch
chmod +x FSearch-x86_64.AppImage
./FSearch-x86_64.AppImage
```

### 2. Verify Checksum

```bash
wget https://github.com/SanmayJoshi/fsearch-appimage/releases/download/continuous/FSearch-x86_64.AppImage.sha256
sha256sum -c FSearch-x86_64.AppImage.sha256
```

### 3. Updating

Because this AppImage includes embedded zsync metadata, you do not need to download the full binary again for updates. Use [appimageupdatetool](https://github.com/AppImageCommunity/AppImageUpdate):

```bash
appimageupdatetool FSearch-x86_64.AppImage
```

---

## How It Works

1. **Check:** A lightweight GitHub Actions job queries the GitHub API for the latest commit on `cboxdoerfer/fsearch:master`.
2. **Comparison:** It compares the commit SHA with the currently deployed continuous release body. If identical, the workflow terminates within seconds.
3. **Build:** If a new commit is detected (or triggered manually via `workflow_dispatch`), the runner builds FSearch from source with Meson/Ninja, bundles GTK runtime dependencies with `linuxdeploy`, generates update information, and updates the `continuous` release tag.

---

## AI / LLM Disclaimer

The CI/CD workflow automation, packaging recipes, and repository documentation were drafted and maintained with the assistance of Artificial Intelligence / Large Language Models (LLMs).

- **Core Application:** The underlying application source code is authored and maintained exclusively by Christian Boxdörfer and upstream contributors.
- **As-Is Basis:** While the pipeline is reviewed to follow AppImage and Linux packaging best practices, the scripts and configurations are provided "as is", without warranty of any kind.

---

## Disclaimer & Credits

- This repository is an **independent, automated community packaging effort** and is not officially affiliated with or endorsed by Christian Boxdörfer.
- All code rights, icons, and trademarks belong to the [FSearch](https://github.com/cboxdoerfer/fsearch) project under the GPL-2.0-or-later license.
- For application bugs or feature requests, visit the [upstream issue tracker](https://github.com/cboxdoerfer/fsearch/issues). For packaging or launch issues specific to this AppImage, open an issue in this repository.

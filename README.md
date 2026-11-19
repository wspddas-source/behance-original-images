# Behance Original Images

Codex skill for downloading every original project image from a Behance
gallery. It extracts images from the rendered project body, maps CDN variants
to Behance's highest-quality `source` version, downloads in parallel, and
verifies that responses are images.

The workflow excludes avatars, related-project covers, tool icons, recommendation
cards, and other page assets.

## Requirements

- Codex in the ChatGPT desktop app with browser automation available for
  gallery inspection.
- Python 3.10 or newer.
- Network access to `mir-s3-cdn-cf.behance.net`.

## Install

Clone the repository directly into the personal Codex skills directory:

```bash
git clone https://github.com/wspddas-source/behance-original-images.git \
  "$HOME/.agents/skills/behance-original-images"
```

Restart Codex if the skill does not appear immediately. The skill is also
compatible with other clients that load the open agent-skills format.

### Install as a plugin

For Plugins Directory installation, add the companion marketplace:

```bash
codex plugin marketplace add wspddas-source/behance-original-images-plugin
```

Restart the ChatGPT desktop app, open the Plugins Directory, select
**wspddas Codex Plugins**, and install **Behance Original Images**.

## Use

In Codex:

```text
Use $behance-original-images to download every original image from this
Behance project into a folder on my Desktop:
https://www.behance.net/gallery/PROJECT_ID/PROJECT_SLUG
```

The skill opens the gallery in the browser, extracts
`/project_modules/` image URLs, and passes them to the downloader.

## Direct script use

The downloader accepts one rendered Behance CDN URL per line on standard input:

```bash
printf '%s\n' \
  'https://mir-s3-cdn-cf.behance.net/project_modules/1400_webp/example.jpg' |
  python3 scripts/download_behance_images.py "$HOME/Desktop/example-project"
```

Options:

```text
--proxy URL      Override HTTP proxy detection
--workers N      Parallel downloads; default: 4
--dry-run        Print source URLs without downloading
--self-test      Run local URL and deduplication checks
```

The script auto-detects the macOS system HTTP proxy. On Linux, Windows, or
custom network setups, set `HTTPS_PROXY` or pass `--proxy`.

## Verification

Use image metadata tools such as `file` and `sips` to verify the downloaded
count, format, and resolution. The script already checks magic bytes and rejects
non-image responses.

## Rights

This repository's MIT license covers the skill instructions and downloader
code. Downloaded images remain subject to the original creator's rights and
Behance's terms. Users are responsible for having permission to use them.

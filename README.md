# Obsidian MD Opener for macOS

Double-click `.md` files in Finder and open them in Obsidian.

This repository provides a small macOS Automator workflow for people who keep Markdown files inside an Obsidian vault and want Finder double-click to reliably open the clicked file in Obsidian.

It is especially useful when tools like Codex generate Markdown files and you want to review them in Obsidian immediately.

## What problem this solves

On macOS, setting `.md` files to open with Obsidian does not always reliably open the clicked file inside the current vault.

This workflow works around that by:

1. Receiving the clicked file path from Finder
2. Converting it to an `obsidian://open?path=...` URL
3. Opening that URL in Obsidian

## Result

After setup, double-clicking a `.md` file in Finder opens that file in Obsidian.

## Requirements

- macOS
- Obsidian installed
- Markdown files stored in a location accessible by Obsidian
- Automator

## Automator script

Create a new **Application** in Automator and add **Run Shell Script**.

Important settings:

- Shell: `/bin/zsh`
- Pass input: `as arguments`

Paste this script:

```bash
for f in "$@"; do
  python3 - "$f" <<'PY'
import sys, os, urllib.parse, subprocess

target = os.path.abspath(sys.argv[1])
url = "obsidian://open?path=" + urllib.parse.quote(target)
subprocess.run(["open", url], check=False)
PY
done

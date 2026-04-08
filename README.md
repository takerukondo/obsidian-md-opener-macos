# Obsidian MD Opener for macOS

[日本語はこちら](./README.ja.md)

A small macOS Automator workflow that lets you double-click `.md` files in Finder and open them in Obsidian.
A small macOS Automator workflow that lets you double-click `.md` files in Finder and open them in Obsidian.

This is useful when Markdown files are generated outside Obsidian, for example by coding tools or AI agents, and you want to review them in Obsidian immediately.

---

## Why this exists

On macOS, even if `.md` files are associated with Obsidian, double-clicking a Markdown file in Finder does not always open that specific file correctly inside Obsidian.

This workflow solves that by passing the clicked file path to Obsidian through an `obsidian://open?path=...` URL.

In short:

**double-click Markdown in Finder → open that file in Obsidian**

---

## What it does

This Automator app:

1. receives the file path from Finder
2. converts it into an Obsidian URL
3. opens the file in Obsidian

---

## Script

Use the following script in an Automator **Application** with:

- Shell: `/bin/zsh`
- Pass input: `as arguments`

```bash
for f in "$@"; do
  python3 - "$f" <<'PY'
import sys, os, urllib.parse, subprocess

target = os.path.abspath(sys.argv[1])
url = "obsidian://open?path=" + urllib.parse.quote(target)
subprocess.run(["open", url], check=False)
PY
done

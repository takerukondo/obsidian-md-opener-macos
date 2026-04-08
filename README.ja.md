# Obsidian MD Opener for macOS

[English version](./README.md)

macOS で `.md` ファイルを Finder からダブルクリックして Obsidian で開くための、小さな Automator ワークフローです。


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

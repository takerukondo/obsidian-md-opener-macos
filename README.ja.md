# Obsidian MD Opener for macOS

[English version](./README.md)

macOS で `.md` ファイルを Finder からダブルクリックして Obsidian で開くための、小さな Automator ワークフローです。

# 目的
macOS では、.md ファイルの既定アプリを Obsidian に設定していても、Finder で Markdown ファイルをダブルクリックしたときに、そのファイル自体がうまく Obsidian 内で開かないことがあります。

このワークフローは、クリックしたファイルのパスを obsidian://open?path=... 形式の URL に変換して Obsidian に渡すことで、その問題を回避します。

Finder で Markdown をダブルクリック → そのファイルを Obsidian で開く

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

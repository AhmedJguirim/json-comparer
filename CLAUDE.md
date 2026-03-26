# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

A self-contained, single-file JSON comparison utility. No build process, no dependencies, no package manager. The entire application lives in `index.html`.

## Running

Open `index.html` directly in a browser. For local development with a server:

```bash
python3 -m http.server 8080
# or
npx serve .
```

## Architecture

Everything is in `index.html` (~140 lines): embedded CSS, HTML structure, and JavaScript.

**Diff algorithm** (`diff` function):
- Recursively walks both JSON objects by sorted keys
- Produces a flat list of `{type, path, left, right}` entries where `type` is `equal | changed | removed | added`
- Recurses into nested plain objects but treats arrays as scalar values (compared via `JSON.stringify`)
- `flatten()` handles the case where a whole subtree is added/removed — it walks the subtree and emits one entry per leaf

**Rendering** (`compare` function):
- Parses both JSON inputs, runs `diff()`, then maps the diff list to parallel `leftLines`/`rightLines` arrays
- Replaces the container innerHTML with two `.diff-view` divs and calls `renderLines()` on each
- Scroll position is synchronized between panes via paired `scroll` event listeners with a flag to prevent feedback loops

**Color scheme**: Catppuccin Mocha (`#1e1e2e` background, `#a6e3a1` green for added, `#f38ba8` red for removed, `#fab387` orange for changed).

## Known Limitations

- Arrays are compared as opaque values (no element-level diffing)
- Keys are always sorted alphabetically in output (original order not preserved)
- No support for comparing JSON arrays at the top level

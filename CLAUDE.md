# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

Roei Herzig's personal academic website, served via GitHub Pages directly from static files at the repo root (no build step, no framework, no package manager). `index.html` is the single-page site; `stylesheet.css` is the only stylesheet.

## Commands

There is no build/lint/test tooling. To preview locally, serve the directory statically on port **8765** (Roei's convention — same port every session, browser bookmark stays valid). Run in the repo root, in the background:

```
python3 -m http.server 8765
```

Then open http://localhost:8765/. If it's already up (curl returns 200), just `open http://localhost:8765/` — no need to restart. Changes are published by committing and pushing to `master` (GitHub Pages serves directly from it).

## Architecture

- **`index.html`** — the entire site. Sections (Highlights, Selected Publications by area, Talks, footer) are plain HTML blocks marked with `<!-- ─────── Section Name ─────── -->` comments. Each publication is a `<li class="paper">` with a `paper-figure` (image/video), `paper-content` (title, authors, venue badge, links, abstract).
- **Show more/less toggling** is done client-side by the inline `<script>` at the bottom of `index.html`. Any `<li>` with class `extra` inside a list is hidden by default; a button with `data-toggle="<list-id>"` reveals it. The button's collapsed label auto-computes from the count of `.extra` items unless `data-collapsed-label` is set explicitly.
- **`stylesheet.css`** — single stylesheet for the whole site (cache-busted via `?v=3` query param in the `<link>` tag in `index.html`; bump this when making CSS changes that must invalidate browser caches).
- **`data/`** — BibTeX (`.bib`) or plain-text (`.txt`) citation files, one per publication, linked from each paper's "bibtex" link.
- **`images/`** — photos, GIFs, and MP4s used as publication figures and profile images.
- **Project subdirectories** (`AG2Video/`, `BrAD/`, `CanonicalSg2Im/`, `DETReg/`, `ORViT/`) — standalone legacy project pages, each with their own `index.html` and `data/`, predating the current unified publications list. Older papers link into these instead of having inline abstracts on the main page.
- **`bio.html`**, **`short-research-statement.html`** — standalone pages linked from the header's "Quick reads" nav, with `.txt` source counterparts.
- **`stylesheet-backup.css`**, **`index_original_copy.html`** — backup copies kept alongside the live files; not referenced by anything live. Don't treat as source of truth.

## Conventions when editing publications

- New papers are prepended to the appropriate `papers-list` (`vrobotics-list` or `vlang-list`), most recent first.
- Mark at most the newest 1-2 papers with `<span class="new-badge">New</span>`; remove the badge from older entries as new ones are added.
- Older items in a list get `class="paper extra"` so they're collapsed under "Show all publications" by default.
- Each entry needs a matching bibtex file added under `data/`.
- Author name matching the site owner is wrapped in `<strong>Roei Herzig</strong>`.

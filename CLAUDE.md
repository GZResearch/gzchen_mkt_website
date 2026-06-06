# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

Personal academic website for Guangzhi Chen (Ph.D. candidate in Quantitative Marketing, University of Florida). Served at **www.guangzhichen.com** via GitHub Pages from the `main` branch of `GZResearch/gzchen_mkt_website`. There is no CI workflow — pushing to `main` deploys. The `CNAME` file pins the custom domain; don't delete it.

## Critical: edit the rendered output directly

The site is **Quarto build output** (`<meta name="generator" content="quarto-1.9.38">`), but the Quarto **source is not in this repo** — there are no `.qmd` files and no `_quarto.yml`. You cannot run `quarto render` here. All content and layout changes are made by **hand-editing the generated HTML/CSS/JSON**. The recent git history (e.g. "Format Teaching section as bullet lists", "Style email link in green") is all direct edits to these files.

Two consequences:
- **`search.json` is Quarto's search index and is not auto-regenerated.** When you change visible text in a page, mirror that change in the matching `objectID`/`section`/`text` entry in `search.json`, or site search goes stale.
- **`site_libs/` is vendored Quarto/Bootstrap assets — do not edit.** Restyle via `styles.css` instead.

## File map

- `index.html` — home page: hero, Research Interests, Research (Working Papers / Publications / Work in Progress, each a `.paper-card`), Teaching. The job-market paper card has classes `paper-card featured` and a `.jmp-flag` badge.
- `cv.html` — CV page: "Download PDF" button plus `assets/cv-page-{1,2,3}.png` previews of the CV.
- `styles.css` — **all custom styling**, linked at the end of each page's `<head>` (after the Quarto Bootstrap bundle, so it overrides it). Not part of `site_libs/`.
- `search.json` — Quarto search index (see above).
- `assets/` — `Guangzhi.jpg` (profile photo), `cv-page-*.png`, `favicon.svg`.
- `*.pdf` (repo root) — downloadable papers/CV linked from the pages.
- `site_libs/` — vendored Quarto runtime (Bootstrap, quarto-nav, clipboard, etc.). Leave alone.

## Styling conventions

`styles.css` defines a small design system on top of Bootstrap:
- CSS custom properties on `:root` drive the palette: `--ink`, `--muted`, `--line`, `--paper`, `--wash`, `--green`/`--green-dark` (links, primary buttons), `--gold` (JMP badge), `--shadow`. Reuse these variables rather than hardcoding colors.
- Component classes own the layout: `.hero` / `.hero-copy` / `.hero-media`, `.paper-card` (+ `.featured`), `.jmp-flag`, `.interest-list` / `.interest-label`, `.availability` (the green "on the job market" pill), `.two-column`, `.panel`, `.cv-page` / `.cv-preview`.
- Quarto's default `#title-block-header` is hidden via CSS; page H1s come from the custom `.hero` / `.cv-header` markup instead.
- Responsive breakpoints at `860px` and `520px` collapse the grids to one column.

## Linking papers and assets (common gotcha)

Page links use repo-root-relative paths, e.g. `href="Guangzhi_Chen_JMP.pdf"`. **A referenced PDF must be committed or the live link 404s.** As of this writing `Guangzhi_Chen_JMP.pdf` and `Guangzhi_Chen_Personalized_Pricing.pdf` are referenced by `index.html` but untracked — they need `git add` before the next deploy. When adding a new paper, drop the PDF in the repo root, link it from the relevant `.paper-card`, and add a matching `search.json` entry.

## Preview locally

No build step. Serve the directory statically and open `index.html`:

```bash
python3 -m http.server 8000   # then visit http://localhost:8000
```

Use a server (not `file://`) so the `site_libs/` script/style paths and `search.json` fetch resolve correctly.

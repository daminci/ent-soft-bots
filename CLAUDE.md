# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Static single-page website for **Ent Soft Bots** (Enterprise Software Robots), a small AI lab. The entire site lives in one self-contained file: `index.html`.

## Architecture

**Single file, no build step.** Everything — markup, styles, and content — is in `index.html`. There are no JavaScript dependencies, no frameworks, no bundler, and no package manager. Opening the file directly in a browser is sufficient to preview it.

External dependencies (loaded via CDN):
- Google Fonts: IBM Plex Mono (headings, nav, monospace elements) + Inter (body copy)

## Design System

All design tokens are CSS custom properties at the top of the `<style>` block:

| Variable   | Value     | Usage                        |
|------------|-----------|------------------------------|
| `--bg`     | `#0a0a0a` | Page background              |
| `--bg-2`   | `#111111` | Card / hover background      |
| `--bg-3`   | `#161616` | Deeper card hover            |
| `--text`   | `#e5e5e5` | Primary text                 |
| `--muted`  | `#888888` | Secondary / subdued text     |
| `--accent` | `#4ade80` | Green accent (links, labels) |
| `--border` | `#222222` | Borders and dividers         |

Typographic rule: `font-family: var(--mono)` for anything identity/technical (nav, headings, labels, tags, counters); `font-family: var(--sans)` for body/descriptive copy.

## Page Sections

Sections in DOM order: `nav` → `#hero` → `#mission` → `#products` → `#team` → `footer`

- **Nav** — fixed, `position: fixed`, blurred backdrop. Logo left, links right.
- **Hero** — headline uses `clamp()` for fluid type sizing.
- **Mission** — 2-column grid (stacks on mobile < 700px); left = prose, right = numbered capability list.
- **Products** — CSS grid with `repeat(auto-fit, minmax(280px, 1fr))`, 6 cards.
- **Team** — similar auto-fit grid, 4 members.
- **Footer** — links + copyright line.

## Workflow

Preview: open `index.html` in any browser — no server needed.

Deploy: the file is fully static; it can be hosted on GitHub Pages, Netlify, Vercel, or any static host with zero configuration.

## Git Discipline

**Commit and push after every meaningful change.** Do not batch unrelated edits into one commit. Each commit should represent one logical unit of work so the history is easy to read and revert.

Commit message format:
- Use a short imperative subject line (e.g. `fix: correct hero heading color`, `feat: add pricing section`)
- Use a type prefix: `feat:`, `fix:`, `style:`, `content:`, or `docs:`
- Add a brief body if the change needs context

```bash
git add index.html          # or CLAUDE.md, etc.
git commit -m "type: short description of what and why"
git push
```

Always push immediately after committing so GitHub stays in sync and changes are never lost locally only.

Remote: `https://github.com/daminci/ent-soft-bots`

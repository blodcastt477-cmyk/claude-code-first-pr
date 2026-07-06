# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Two single-page static websites, plus the repo's original purpose as a practice
ground for the GitHub + Claude Code PR workflow (clone, branch, commit, push, PR):

- `index.html` — KUBINTANG, a site for Koperasi Usahawan Bukit Bintang Berhad,
  a Malaysian cooperative.
- `optical.html` — D'EYEWEAR, an independent optician shop in SS2, Petaling Jaya
  (real business details: address, phone 03-7877 9576, info@deyewear.com).
  Frame prices on this page are placeholders, not confirmed by the owner.

Site work happens on feature branches (currently `feature/koperasi-website`)
with PRs into `main`. `main` contains only the README.

## Deployment

Both pages are live on GitHub Pages, served from the **`feature/koperasi-website`
branch root** (not `main`):

- https://blodcastt477-cmyk.github.io/claude-code-first-pr/ (KUBINTANG)
- https://blodcastt477-cmyk.github.io/claude-code-first-pr/optical.html (D'EYEWEAR)

Pushing to `feature/koperasi-website` triggers a Pages redeploy (takes a minute
or two). First-time Pages deploys have failed transiently before; re-request a
build with `gh api -X POST repos/blodcastt477-cmyk/claude-code-first-pr/pages/builds`.

## Running Locally

There is no build, lint, or test tooling — both sites are plain HTML/CSS/JS with
no dependencies. Serve locally with the preconfigured dev server (defined in
`.claude/launch.json`):

- Preview tool: `preview_start` with the `static-site` config
- Equivalent manual command: `python -m http.server 5500`

Serve over HTTP rather than opening files directly, since pages load images by
relative path and Google Fonts.

## Architecture

Each site is one self-contained HTML file: CSS in a single `<style>` block in
`<head>`, markup, then all JS in one `<script>` block at the end of `<body>`.
The only binary assets are photos in `images/` (used by `index.html` only;
`optical.html` draws all graphics as inline SVG). Keep this single-file
structure — don't split out CSS/JS files.

### index.html (KUBINTANG)

- Colors as CSS variables in `:root` (`--obsidian`, `--gold`, `--bone`) — use
  the variables, not hardcoded hex. Fonts: Italiana (headings), Jost (body),
  IBM Plex Mono (eyebrows/labels).
- Desktop nav `<ul>` and `.mobile-menu` duplicate the same links — nav changes
  must be made in both.
- Scroll-driven clip-path reveal (`#scrollReveal`): a sticky section whose
  `clip-path` and `background-size` are interpolated from `window.scrollY`;
  constants at the top of the `scrollReveal()` IIFE.
- Contact form is front-end only (no backend).

### optical.html (D'EYEWEAR)

- Colors in `:root`: ink on cool optical-white, with a green→violet
  "anti-reflective coating flare" gradient (`--flare`) as the only accent.
  Fonts: Bricolage Grotesque (display), Instrument Sans (body), Fragment Mono
  (prescription-style data).
- Signature interaction: the hero headline is blurred with chromatic fringing;
  a cursor-following lens (`.stage-sharp` clipped to a circle + `.lens-ring`)
  reveals sharp text. Blur and sharp layers duplicate the same copy — edit both.
  Touch devices get an auto-drifting lens; reduced motion disables the effect
  via the `.lens-off` class.
- Other graphic set pieces: prescription ticker, SVG frame illustrations with
  real temple measurements (e.g. `47□22–145`), lens cross-section diagram,
  Snellen eye chart where copy shrinks by acuity row, red/green duochrome strip.

### Shared conventions

Both pages support `prefers-reduced-motion` in CSS and JS, use
IntersectionObserver-driven `.reveal` → `.in` scroll fades, and keep
`:focus-visible` outlines. Preserve these when adding animated or interactive
features.

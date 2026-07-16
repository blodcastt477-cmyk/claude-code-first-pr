# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

A single-page static website, plus the repo's original purpose as a practice
ground for the GitHub + Claude Code PR workflow (clone, branch, commit, push, PR):

- `index.html` — KUBINTANG, a site for Koperasi Usahawan Bukit Bintang Berhad,
  a Malaysian cooperative.

A second site, D'EYEWEAR (an optician in SS2, Petaling Jaya), was built here
but now lives in its own repo `blodcastt477-cmyk/deyewear`, deployed to
https://deyewear.store. Edit that repo for D'EYEWEAR changes. Its frame prices
are placeholders, not confirmed by the owner.

Site work happens on feature branches (currently `feature/koperasi-website`)
with PRs into `main`. `main` contains only the README.

## Deployment

This repo's GitHub Pages site is served from the **`feature/koperasi-website`
branch root** (not `main`) at the custom domain **https://kubintang.com** (the
committed `CNAME` file). Do not change or remove `CNAME` — one Pages site
supports exactly one custom domain.

D'EYEWEAR is deployed separately: repo `blodcastt477-cmyk/deyewear` (branch
`main`, its own `CNAME`) serves **https://deyewear.store**.

Pushing to the published branch triggers a Pages redeploy (takes a minute or
two). First-time Pages deploys have failed transiently before; re-request a
build with `gh api -X POST repos/blodcastt477-cmyk/<repo>/pages/builds`.

## Running Locally

There is no build, lint, or test tooling — both sites are plain HTML/CSS/JS with
no dependencies. Serve locally with the preconfigured dev server (defined in
`.claude/launch.json`):

- Preview tool: `preview_start` with the `static-site` config
- Equivalent manual command: `python -m http.server 5500`

Serve over HTTP rather than opening files directly, since pages load images by
relative path and Google Fonts.

## Architecture

The site is one self-contained HTML file: CSS in a single `<style>` block in
`<head>`, markup, then all JS in one `<script>` block at the end of `<body>`.
The only binary assets are photos in `images/`. Keep this single-file
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

### Conventions

The page supports `prefers-reduced-motion` in CSS and JS, uses
IntersectionObserver-driven `.reveal` → `.in` scroll fades, and keeps
`:focus-visible` outlines. Preserve these when adding animated or interactive
features.

# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is the Astro static site for **Riva Refuge** (www.rivarefuge.org), a small secular non-profit. Repo: `RivaRefuge/riva-refuge-website`. Deployed to GitHub Pages via GitHub Actions on every push to `master`.

## Commands

```bash
npm install                 # Install dependencies
npm run dev                 # Serve locally at http://localhost:4321/
npm run build               # Build to dist/
npm run preview             # Preview the dist/ build locally
```

## Architecture

The site is a self-contained one-pager. There is no shared layout component, no nav/footer component, and no CSS framework. Each page is a standalone `.astro` file that includes its own `<html>`, `<head>`, and `<body>`.

### File Structure

```
src/
  assets/
    images/                 # All site images — processed to WebP at build time
  pages/
    index.astro             # One-pager homepage (self-contained)
    404.astro               # Standalone 404 page
    thanks/index.astro      # PayPal donate return page
public/
  css/rr-custom.css         # Only local stylesheet
  CNAME                     # Custom domain: www.rivarefuge.org
  robots.txt
  favicon.ico / favicon.svg
```

### Images

Images live in `src/assets/images/` and are imported at the top of each page:

```astro
---
import { Image } from 'astro:assets';
import hero from '../assets/images/fp-hero.jpg';
---
<Image src={hero} alt="Description" />
```

- Astro converts images to WebP and infers dimensions automatically
- Hero/first images: add `width={1400} loading="eager" fetchpriority="high"`
- All other images: no extra attributes needed (lazy loading is the default)

### Styling

- Custom styles only — no CSS framework
- Stylesheet: `public/css/rr-custom.css`
- Font Awesome 6.4.0 via cdnjs CDN (linked in each page's `<head>`)
- Cloudflare Web Analytics beacon included in each page's `<body>`

### Deployment

GitHub Actions workflow (`.github/workflows/jekyll-gh-pages.yml`) runs `npm ci && npm run build` and deploys `dist/` to GitHub Pages on every push to `master`.

# Beta-to-Production Promotion — Design Spec

**Date:** 2026-03-17
**Status:** Approved
**Summary:** Promote the beta one-pager to become the production homepage, remove all old multi-page content and shared components, and rewrite the thanks and 404 pages to match the new design.

## Context

The site has a redesigned one-pager living at `/beta/` (`src/pages/beta/index.astro`) with its own self-contained CSS (`public/css/beta.css`). User feedback has approved this design. The old multi-page site (using `Layout.astro`, `Nav.astro`, `Footer.astro`, `DonateButton.astro`, Bootstrap, and `rr-custom.css`) is being retired.

Everything deleted is recoverable from git history.

## Changes

### 1. Homepage Promotion

Replace `src/pages/index.astro` with the beta one-pager content, with these adjustments:

- Remove `<meta name="robots" content="noindex, nofollow" />`
- Change canonical URL from `/beta/` to `/`
- Change stylesheet reference from `/css/beta.css` to `/css/rr-custom.css`
- Add Cloudflare Web Analytics beacon: `<script defer src="https://static.cloudflareinsights.com/beacon.min.js" data-cf-beacon='{"token": "f2b00cbfb5a94b04b09d69db7c5524a5"}'></script>`
- All other content (nav, hero, sections, scripts) stays as-is

### 2. Stylesheet Swap

- Replace `public/css/rr-custom.css` with the contents of `public/css/beta.css`
- Delete `public/css/beta.css`

### 3. Thanks Page Rewrite

Rewrite `src/pages/thanks/index.astro` as a standalone page (no `Layout.astro`):

- Uses `/css/rr-custom.css` and Font Awesome CDN (same as homepage)
- Includes: nav brand (logo + "Riva Refuge"), thank-you message, link back to homepage
- No full nav menu or footer — lightweight standalone page
- Keeps hero image for visual continuity
- Add `<meta name="robots" content="noindex" />` to keep it out of the sitemap/search
- Add Cloudflare Web Analytics beacon
- Exists to support PayPal donate flow return URL (`/thanks/`) — with the direct PayPal link (not SDK), the return redirect depends on the PayPal hosted button settings; this page is kept as a best-effort landing target

### 4. 404 Page Rewrite

Rewrite `src/pages/404.astro` as a standalone page (no `Layout.astro`):

- Same lightweight approach as the thanks page
- Uses `/css/rr-custom.css` and Font Awesome CDN
- Includes: nav brand, "page not found" message, link back to homepage
- Add Cloudflare Web Analytics beacon

### 5. Deletions

**Pages** (content now covered by the one-pager):
- `src/pages/beta/index.astro`
- `src/pages/campaigns/index.astro`
- `src/pages/campaigns/jadelle-family-planning-program/index.astro`
- `src/pages/campaigns/scholarship-program/index.astro`
- `src/pages/campaigns/matisi-food-medicine/index.astro`
- `src/pages/our-history/index.astro`
- `src/pages/our-vision/index.astro`
- `src/pages/board-of-directors/index.astro`
- `src/pages/contact-us/index.astro`
- `src/pages/get-involved/index.astro`

**Shared components** (no longer used):
- `src/layouts/Layout.astro`
- `src/components/Nav.astro`
- `src/components/Footer.astro`
- `src/components/DonateButton.astro`

**CSS:**
- `public/css/beta.css` (after contents moved to `rr-custom.css`)

### 6. Update CLAUDE.md

Rewrite `CLAUDE.md` to reflect the new single-page architecture:

- Remove references to Layout props, menuEntry, Nav.astro, Footer.astro, DonateButton.astro
- Update file structure to show the simplified layout (index, thanks, 404)
- Document that the site is a self-contained one-pager with its own CSS
- Update "Adding a New Page" guidance or remove if no longer applicable

### Files Kept (unchanged)

- `src/pages/thanks/index.astro` (rewritten, not deleted)
- `src/pages/404.astro` (rewritten, not deleted)
- `public/CNAME`, `public/robots.txt`, `public/favicon.*`
- All images in `src/assets/images/`
- `.github/workflows/jekyll-gh-pages.yml`
- `astro.config.mjs`, `package.json`, etc.

## Out of Scope

- Redirects from old URLs — not needed for this small org site
- Changes to the deployment workflow
- Any new content or features

# Beta-to-Production Promotion Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Replace the multi-page site with the approved beta one-pager, clean up all old components, and update documentation.

**Architecture:** The beta one-pager (`src/pages/beta/index.astro`) becomes the homepage (`src/pages/index.astro`). The old shared component stack (Layout, Nav, Footer, DonateButton) and all content pages are deleted. The thanks and 404 pages are rewritten as lightweight standalone pages matching the new design. `beta.css` replaces `rr-custom.css`.

**Tech Stack:** Astro, CSS, Font Awesome 6.4.0 (CDN), Cloudflare Web Analytics

**Spec:** `docs/superpowers/specs/2026-03-17-beta-promotion-design.md`

---

### Task 1: Swap the stylesheet

**Files:**
- Overwrite: `public/css/rr-custom.css` (with contents of `public/css/beta.css`)
- Delete: `public/css/beta.css`

- [ ] **Step 1: Replace rr-custom.css with beta.css contents**

Copy the full contents of `public/css/beta.css` into `public/css/rr-custom.css`, replacing everything.

- [ ] **Step 2: Delete beta.css**

```bash
rm public/css/beta.css
```

- [ ] **Step 3: Commit**

```bash
git add public/css/rr-custom.css
git rm public/css/beta.css
git commit -m "Replace rr-custom.css with beta styles"
```

---

### Task 2: Promote beta to homepage

**Files:**
- Overwrite: `src/pages/index.astro` (with adjusted beta content)

- [ ] **Step 1: Replace index.astro with beta content**

Copy the full contents of `src/pages/beta/index.astro` into `src/pages/index.astro`, with these changes:

1. Update image import paths (one level up instead of two):
   - `../../assets/images/` → `../assets/images/`

2. Remove the noindex meta tag:
   ```html
   <!-- DELETE THIS LINE -->
   <meta name="robots" content="noindex, nofollow" />
   ```

3. Change canonical URL:
   ```js
   // FROM:
   const canonicalURL = new URL('/beta/', 'https://www.rivarefuge.org');
   // TO:
   const canonicalURL = new URL('/', 'https://www.rivarefuge.org');
   ```

4. Change stylesheet reference:
   ```html
   <!-- FROM: -->
   <link rel="stylesheet" href="/css/beta.css" />
   <!-- TO: -->
   <link rel="stylesheet" href="/css/rr-custom.css" />
   ```

5. Add Cloudflare Web Analytics beacon before `</body>`:
   ```html
   <script defer src="https://static.cloudflareinsights.com/beacon.min.js" data-cf-beacon='{"token": "f2b00cbfb5a94b04b09d69db7c5524a5"}'></script>
   ```

- [ ] **Step 2: Verify the build succeeds**

```bash
npm run build
```

Expected: Clean build with no errors.

- [ ] **Step 3: Commit**

```bash
git add src/pages/index.astro
git commit -m "Promote beta one-pager to homepage"
```

---

### Task 3: Rewrite thanks page

**Files:**
- Overwrite: `src/pages/thanks/index.astro`

- [ ] **Step 1: Rewrite thanks page as standalone**

Replace `src/pages/thanks/index.astro` with this content:

```astro
---
import { Image } from 'astro:assets';
import logoImg from '../../assets/images/rrlogo-only.png';
import heroImg from '../../assets/images/fp-hero.jpg';

const title = "Thank You | Riva Refuge";
---

<!doctype html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>{title}</title>
  <meta name="robots" content="noindex" />
  <link rel="icon" type="image/svg+xml" href="/favicon.svg" />
  <link rel="icon" type="image/x-icon" href="/favicon.ico" />
  <link rel="stylesheet" href="/css/rr-custom.css" />
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css"
    integrity="sha512-iecdLmaskl0HPNiecjHEM7FnGQAU1e3tOU/WnJiB8JPA5aafDEhTuMFBm5JZef+IehljEmW5SDLRI+iH+LFfA=="
    crossorigin="anonymous" referrerpolicy="no-referrer" />
</head>
<body>

<nav class="beta-nav">
  <div class="beta-nav-inner">
    <a href="/" class="beta-nav-brand">
      <Image src={logoImg} alt="" width={38} loading="eager" />
      <span><span class="brand-riva">Riva</span> <span class="brand-refuge">Refuge</span></span>
    </a>
  </div>
</nav>

<section class="beta-hero" style="min-height:60vh;">
  <div class="beta-hero-bg">
    <Image src={heroImg} alt="Community in Kenya" width={1400} loading="eager" fetchpriority="high" />
  </div>
  <div class="beta-hero-content">
    <h1>Thank You</h1>
    <p class="beta-hero-tagline">
      Riva Refuge thanks you for your generous donation.
      Every dollar goes directly to the people who need it.
    </p>
    <a href="/" class="btn-hero">Back to Home</a>
  </div>
</section>

<script defer src="https://static.cloudflareinsights.com/beacon.min.js" data-cf-beacon='{"token": "f2b00cbfb5a94b04b09d69db7c5524a5"}'></script>
</body>
</html>
```

- [ ] **Step 2: Verify the build succeeds**

```bash
npm run build
```

Expected: Clean build, `/thanks/` route exists in `dist/`.

- [ ] **Step 3: Commit**

```bash
git add src/pages/thanks/index.astro
git commit -m "Rewrite thanks page as standalone with new design"
```

---

### Task 4: Rewrite 404 page

**Files:**
- Overwrite: `src/pages/404.astro`

- [ ] **Step 1: Rewrite 404 page as standalone**

Replace `src/pages/404.astro` with this content:

```astro
---
import { Image } from 'astro:assets';
import logoImg from '../assets/images/rrlogo-only.png';

const title = "Page Not Found | Riva Refuge";
---

<!doctype html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>{title}</title>
  <link rel="icon" type="image/svg+xml" href="/favicon.svg" />
  <link rel="icon" type="image/x-icon" href="/favicon.ico" />
  <link rel="stylesheet" href="/css/rr-custom.css" />
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css"
    integrity="sha512-iecdLmaskl0HPNiecjHEM7FnGQAU1e3tOU/WnJiB8JPA5aafDEhTuMFBm5JZef+IehljEmW5SDLRI+iH+LFfA=="
    crossorigin="anonymous" referrerpolicy="no-referrer" />
</head>
<body>

<nav class="beta-nav">
  <div class="beta-nav-inner">
    <a href="/" class="beta-nav-brand">
      <Image src={logoImg} alt="" width={38} loading="eager" />
      <span><span class="brand-riva">Riva</span> <span class="brand-refuge">Refuge</span></span>
    </a>
  </div>
</nav>

<section class="beta-hero" style="min-height:60vh;">
  <div class="beta-hero-bg" style="background:var(--rr-dark);"></div>
  <div class="beta-hero-content">
    <h1>Page Not Found</h1>
    <p class="beta-hero-tagline">
      Sorry, we couldn't find the page you're looking for.
    </p>
    <a href="/" class="btn-hero">Back to Home</a>
  </div>
</section>

<script defer src="https://static.cloudflareinsights.com/beacon.min.js" data-cf-beacon='{"token": "f2b00cbfb5a94b04b09d69db7c5524a5"}'></script>
</body>
</html>
```

- [ ] **Step 2: Verify the build succeeds**

```bash
npm run build
```

Expected: Clean build, `dist/404.html` exists.

- [ ] **Step 3: Commit**

```bash
git add src/pages/404.astro
git commit -m "Rewrite 404 page as standalone with new design"
```

---

### Task 5: Delete old pages

**Files:**
- Delete: `src/pages/beta/index.astro`
- Delete: `src/pages/campaigns/index.astro`
- Delete: `src/pages/campaigns/jadelle-family-planning-program/index.astro`
- Delete: `src/pages/campaigns/scholarship-program/index.astro`
- Delete: `src/pages/campaigns/matisi-food-medicine/index.astro`
- Delete: `src/pages/our-history/index.astro`
- Delete: `src/pages/our-vision/index.astro`
- Delete: `src/pages/board-of-directors/index.astro`
- Delete: `src/pages/contact-us/index.astro`
- Delete: `src/pages/get-involved/index.astro`

- [ ] **Step 1: Delete all old content pages**

```bash
rm src/pages/beta/index.astro
rm src/pages/campaigns/index.astro
rm src/pages/campaigns/jadelle-family-planning-program/index.astro
rm src/pages/campaigns/scholarship-program/index.astro
rm src/pages/campaigns/matisi-food-medicine/index.astro
rm src/pages/our-history/index.astro
rm src/pages/our-vision/index.astro
rm src/pages/board-of-directors/index.astro
rm src/pages/contact-us/index.astro
rm src/pages/get-involved/index.astro
```

- [ ] **Step 2: Remove empty directories**

```bash
rmdir src/pages/beta
rmdir src/pages/campaigns/jadelle-family-planning-program
rmdir src/pages/campaigns/scholarship-program
rmdir src/pages/campaigns/matisi-food-medicine
rmdir src/pages/campaigns
rmdir src/pages/our-history
rmdir src/pages/our-vision
rmdir src/pages/board-of-directors
rmdir src/pages/contact-us
rmdir src/pages/get-involved
```

- [ ] **Step 3: Commit**

```bash
git add -A src/pages/
git commit -m "Remove old multi-page content (now covered by one-pager)"
```

---

### Task 6: Delete old shared components

**Files:**
- Delete: `src/layouts/Layout.astro`
- Delete: `src/components/Nav.astro`
- Delete: `src/components/Footer.astro`
- Delete: `src/components/DonateButton.astro`

- [ ] **Step 1: Delete all old shared components**

```bash
rm src/layouts/Layout.astro
rm src/components/Nav.astro
rm src/components/Footer.astro
rm src/components/DonateButton.astro
```

- [ ] **Step 2: Remove empty directories**

```bash
rmdir src/layouts
rmdir src/components
```

- [ ] **Step 3: Verify the build still succeeds**

```bash
npm run build
```

Expected: Clean build. No remaining references to deleted files.

- [ ] **Step 4: Commit**

```bash
git add -A src/layouts/ src/components/
git commit -m "Remove old Layout, Nav, Footer, and DonateButton components"
```

---

### Task 7: Update CLAUDE.md

**Files:**
- Modify: `CLAUDE.md`

- [ ] **Step 1: Rewrite CLAUDE.md**

Update `CLAUDE.md` to reflect the new architecture. Key changes:

1. **File Structure** — Replace the full tree with:
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

2. **Architecture section** — Remove references to:
   - Layout props (`title`, `menuEntry`, `description`)
   - Shared components (`Nav.astro`, `Footer.astro`, `DonateButton.astro`)
   - `Layout.astro` wrapper
   - Bootstrap CDN

3. **Architecture section** — Add description of:
   - Self-contained one-pager architecture
   - Pages are standalone HTML (no shared layout)
   - Custom CSS in `rr-custom.css` (no framework)
   - Font Awesome 6.4.0 via CDN
   - Cloudflare Web Analytics beacon

4. **Remove "Adding a New Page"** section (or simplify to note that pages are standalone)

5. **Keep unchanged:** Project Overview, Commands, Images, Styling (update to remove Bootstrap reference), Deployment

- [ ] **Step 2: Verify accuracy**

Confirm CLAUDE.md matches the actual file structure after all deletions.

- [ ] **Step 3: Commit**

```bash
git add CLAUDE.md
git commit -m "Update CLAUDE.md for one-pager architecture"
```

---

### Task 8: Final verification

- [ ] **Step 1: Clean build**

```bash
rm -rf dist && npm run build
```

Expected: Clean build with no errors or warnings about missing files.

- [ ] **Step 2: Verify output structure**

```bash
ls dist/
ls dist/thanks/
ls dist/404.html
```

Expected: `index.html` at root, `thanks/index.html`, and `404.html` all exist.

- [ ] **Step 3: Local preview**

```bash
npm run preview
```

Manually check:
- Homepage loads with the one-pager design at `http://localhost:4321/`
- `/thanks/` shows the thank-you page with matching design
- A non-existent URL shows the 404 page
- Cloudflare beacon script is present in page source for all three pages

- [ ] **Step 4: Verify no orphaned image imports**

```bash
grep -r "from.*assets/images" src/pages/
```

Expected: Only imports from `index.astro`, `thanks/index.astro`, and `404.astro` — no broken references.

# Visual Refresh — Design Spec

**Date:** 2026-04-13
**Direction:** Light base + bold typography + subtle textures (C+D merged, option B warm variant)
**Scope:** CSS changes + minor HTML tweaks to `index.html` and `work.html`. No new dependencies, no build step.

## Design Principles

- **Fully light site** — no dark sections. The page reads as one unified light canvas.
- **Bold typography carries visual weight** — heavy font-weight (800), near-black color (#1a1a1a), tight letter-spacing on headings.
- **Dot grid texture** on every section — subtle `radial-gradient` pattern at 28px spacing, low opacity (~0.05). Adds visual interest without being distracting.
- **Geometric watermarks** — low-opacity crimson rounded-rectangles and teal circles placed as background decorations on key sections. Never foreground elements.
- **Crimson (#C0392B) and teal (#2A9D8F)** used as precise accent colors — underlines, labels, dot separators, company names. Not backgrounds.
- **Gradient bar** — a thin (3px) crimson→teal gradient line at the very top of the page, acting as a brand signature.

## Color Palette (Updated)

| Role | Old | New |
|------|-----|-----|
| Page background | `#FAFAFA` | `#FAFAFA` (unchanged) |
| Hero background | `#3D3D3D` (dark) | `#f5f3f0` (warm light) |
| Projects section bg | `#3D3D3D` (dark) | `#FAFAFA` (light) |
| Skills banner bg | `#3D3D3D` (dark) | `#f0f0f0` (light gray) |
| Heading text | `#3D3D3D` | `#1a1a1a` (darker, more contrast) |
| Body text | `#3D3D3D` | `#444` (slightly lighter for hierarchy) |
| Secondary text | — | `rgba(0,0,0,0.5)` |
| Accent 1 (crimson) | `#C0392B` | `#C0392B` (unchanged) |
| Accent 2 (teal) | `#2A9D8F` | `#2A9D8F` (unchanged) |

## Section-by-Section Changes

### 1. Global / Page Level

**CSS changes:**
- Add `::before` pseudo-element on `body` or sections for the dot grid texture:
  ```
  background-image: radial-gradient(circle at 1px 1px, rgba(0,0,0,0.05) 1px, transparent 0);
  background-size: 28px 28px;
  ```
- Add a 3px gradient bar at the very top of the page (on `body::before` or a new element):
  ```
  background: linear-gradient(90deg, #C0392B, #2A9D8F);
  height: 3px;
  ```
- Update CSS variables:
  - `--colors--first-color: #1a1a1a` (was `#3D3D3D`)
  - `--colors--fourth-color: #1a1a1a` (was `#3D3D3D`)
- Add `--font-weight--800: 800` for bold headings

### 2. Hero Section

**CSS changes:**
- `.hero` background: `#3D3D3D` → `#f5f3f0`
- `.hero` color: `#FAFAFA` → `#1a1a1a`
- `.title-1` font-weight: 600 → 800
- `.title-1` color: inherit from parent (now dark text on light bg)
- `.heading-overlay`, `.heading-overlay-2` background: update to match new hero bg `#f5f3f0`
- `.container-bottom` text colors: update to dark variants
- `.top-text` color: `rgba(0,0,0,0.5)` (was light)
- `.social-circle` backgrounds: review for contrast on light bg
- `.globe` filter: remove `invert()` (no longer needed on light bg)
- Add geometric watermarks via `::after` pseudo-elements on `.hero`

**HTML changes (`index.html`):**
- Add tagline paragraph after the hero-flex heading block:
  ```
  Creative technologist building at the intersection of design and code.
  Full-stack dev, CG artist, and motion designer based in Singapore.
  ```
- Add skill labels below tagline:
  ```
  DEV (crimson underline) · ART (teal underline) · MOTION (gray underline)
  ```
- Remove the `[data-w-id] { opacity: 1 !important; }` override if Webflow animations still work (test this)

### 3. Profile Photo Section

**CSS changes:**
- Add dot grid texture to `.section-photo.video` background
- Keep existing `.project-wrapper-link` border-part frame effect
- No color changes needed (already light)

### 4. About / Experience / Achievements Section

**CSS changes:**
- `.section.gray` already has `#FAFAFA` bg — add dot grid texture
- `.page-title` headings: bump weight to 800, color to `#1a1a1a`
- Add subtle geometric watermark via `::after` on `.section.gray`
- Experience company names (the middle column in `.text-flex`): color `#C0392B`
- `.line-divider-award .line-absolute`: keep but ensure it's `rgba(0,0,0,0.08)` for subtlety

**HTML changes:**
- None required — existing structure works

### 5. Project Cards Section

**CSS changes:**
- `.section` (the work section): background `#3D3D3D` → `#FAFAFA`
- `.section` color: `#FAFAFA` → `#1a1a1a`
- `.page-title.flex` in work section: color `#1a1a1a`, weight 800
- `.project-photo._01`: add `border-radius: 8px`
- `.project-small-title.biger`: color `#1a1a1a`
- `.project-small-title.lighter`: color `#999`
- `.page-divider`: update color for light bg visibility

**HTML changes:**
- None required for now (placeholder content will be replaced later as a separate task)

### 6. Skills Banner Section

**CSS changes:**
- `.section-cta.for-services`: background `#3D3D3D` → `#f0f0f0`
- `.text-rotator-big.for-services`: color `#FAFAFA` → `#1a1a1a`
- `.happy-face-hero._130px`: replace circle with colored dot separator
  - Alternating `#C0392B` and `#2A9D8F` backgrounds
  - Reduce size to ~12px dot
- Add dot grid texture

### 7. Footer

**CSS changes:**
- `.footer-main` already light — add dot grid texture
- `.page-title.flex.second` (CTA heading): weight 800, color `#1a1a1a`
- `.page-title.flex.third` (section labels): add `text-transform: uppercase`, `letter-spacing: 1px`, `font-size: 11px`, color `#999`
- `.button-text` in footer: ensure dark text color
- Email button: crimson underline style

**HTML changes:**
- None required (template links already removed in prior fix)

### 8. Work Page (`work.html`)

**CSS changes apply globally** — work.html shares the same stylesheet, so all section-level changes propagate automatically. Specific changes:
- The work page uses `.section.with-work-page` which also inherits from `.section` — verify it picks up the light background
- Footer changes carry over

## Files Modified

1. `css/mingkey88-com.webflow.css` — all CSS changes (variables, section backgrounds, typography, textures, watermarks)
2. `index.html` — hero tagline + skill labels, gradient bar element
3. `work.html` — gradient bar element (if not handled globally via CSS)

## What This Does NOT Change

- Font families (DM Sans, Cormorant Garamond, Caveat stay)
- Page structure / layout grid
- Webflow animation system (`webflow.js`, `data-w-id` attributes)
- Scroll reveal animations
- Navigation menu behavior
- Project card content (placeholder replacement is a separate task)
- Profile photo
- Social links (already fixed)
- SEO / meta tags / structured data

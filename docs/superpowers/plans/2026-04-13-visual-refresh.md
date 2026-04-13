# Visual Refresh Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Transform the portfolio from a flat, dark Webflow template into a unified light site with bold typography, dot grid textures, geometric watermarks, and crimson/teal accent colors.

**Architecture:** All changes are CSS overrides appended to the existing `mingkey88-com.webflow.css` plus minor HTML edits in `index.html` and `work.html`. No new dependencies. The Webflow base styles stay intact — we override specific selectors at the end of the CSS file.

**Tech Stack:** Static HTML/CSS. No build step. Preview with `python -m http.server 8000`.

**Spec:** `docs/superpowers/specs/2026-04-13-visual-refresh-design.md`

---

## File Map

| File | Action | Responsibility |
|------|--------|----------------|
| `css/mingkey88-com.webflow.css` | Modify (append overrides at end) | All visual changes — colors, textures, typography |
| `index.html` | Modify | Gradient bar, hero tagline + skill labels, skills banner dot separators |
| `work.html` | Modify | Gradient bar |

All CSS changes go in a single clearly-marked override block at the end of the existing stylesheet. This avoids conflicts with the Webflow base styles and makes changes easy to find/revert.

---

### Task 1: CSS Variables & Global Dot Grid Texture

**Files:**
- Modify: `css/mingkey88-com.webflow.css:1-50` (`:root` block) and append after line ~3016

- [ ] **Step 1: Update CSS variables in `:root`**

In `css/mingkey88-com.webflow.css`, change these values in the existing `:root` block (lines 1-50):

```css
/* Line 4: was #3D3D3D */
--colors--fourth-color: #1a1a1a;

/* Line 18: was #3D3D3D */
--colors--first-color: #1a1a1a;

/* Line 26: was #3D3D3D */
--color--first-color: #1a1a1a;

/* Line 32: was rgba(61, 61, 61, 0.5) */
--colors--third-color: rgba(26, 26, 26, 0.5);
```

- [ ] **Step 2: Append the visual refresh override block**

At the very end of `css/mingkey88-com.webflow.css` (after line ~3016), append a comment banner and the dot grid utility class:

```css
/* ============================================
   VISUAL REFRESH OVERRIDES — 2026-04-13
   Light base, bold typography, dot grid texture,
   geometric watermarks, crimson/teal accents.
   ============================================ */

/* --- Dot grid texture (applied per-section) --- */
.dot-grid::before {
  content: "";
  position: absolute;
  inset: 0;
  background-image: radial-gradient(circle at 1px 1px, rgba(0,0,0,0.05) 1px, transparent 0);
  background-size: 28px 28px;
  pointer-events: none;
  z-index: 0;
}
```

- [ ] **Step 3: Verify the file saves correctly**

Run: `python -m http.server 8000` and open `http://localhost:8000` in a browser.
Expected: Site loads without CSS errors. No visual change yet (dot-grid class not applied to any elements).

- [ ] **Step 4: Commit**

```bash
git add css/mingkey88-com.webflow.css
git commit -m "refactor: update CSS variables to darker text, add dot-grid utility"
```

---

### Task 2: Gradient Bar & Hero — Light Background

**Files:**
- Modify: `index.html:23-28` (inline styles), `index.html:106-144` (hero section)
- Modify: `work.html:23-25` (inline styles), `work.html:36` (after body open)
- Modify: `css/mingkey88-com.webflow.css` (append to override block)

- [ ] **Step 1: Add gradient bar CSS**

Append to the override block in `css/mingkey88-com.webflow.css`:

```css
/* --- Gradient bar at top of page --- */
.gradient-bar {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  height: 3px;
  background: linear-gradient(90deg, #C0392B, #2A9D8F);
  z-index: 10000;
}

/* --- Hero: light background --- */
.hero {
  background-color: #f5f3f0;
  color: #1a1a1a;
}

.heading-overlay,
.heading-overlay-2 {
  background-color: #f5f3f0;
}

/* Hero geometric watermarks */
.hero::before {
  content: "";
  position: absolute;
  top: -60px;
  right: -40px;
  width: 280px;
  height: 280px;
  background: linear-gradient(135deg, #C0392B, #e74c3c);
  border-radius: 20px;
  transform: rotate(15deg);
  opacity: 0.08;
  pointer-events: none;
}

.hero::after {
  content: "";
  position: absolute;
  bottom: -80px;
  left: -40px;
  width: 240px;
  height: 240px;
  background: linear-gradient(135deg, #2A9D8F, #45b7aa);
  border-radius: 50%;
  opacity: 0.07;
  pointer-events: none;
}

/* Hero text colors for light bg */
.top-text {
  color: rgba(0,0,0,0.5);
}

.globe {
  filter: none;
}

.social-circle.linkdin,
.social-circle.contra,
.social-circle.instagram {
  opacity: 0.85;
}

/* Hero tagline */
.hero-tagline {
  font-size: 1rem;
  color: rgba(0,0,0,0.5);
  max-width: 480px;
  line-height: 1.5;
  font-weight: 400;
  margin-top: 20px;
}

/* Hero skill labels */
.hero-skills {
  display: flex;
  gap: 12px;
  margin-top: 20px;
}

.hero-skills .skill-label {
  font-size: 11px;
  letter-spacing: 2px;
  font-weight: 600;
  padding-bottom: 2px;
}

.hero-skills .skill-label.dev {
  color: #C0392B;
  border-bottom: 1px solid #C0392B;
}

.hero-skills .skill-label.art {
  color: #2A9D8F;
  border-bottom: 1px solid #2A9D8F;
}

.hero-skills .skill-label.motion {
  color: #999;
  border-bottom: 1px solid #bbb;
}
```

- [ ] **Step 2: Add gradient bar HTML to `index.html`**

In `index.html`, immediately after `<body>` (line 57), before the skip-link, add:

```html
  <div class="gradient-bar" aria-hidden="true"></div>
```

- [ ] **Step 3: Add hero tagline and skill labels to `index.html`**

In `index.html`, after the closing `</div>` of `.hero-flex` (line 119, after `</div><!-- end hero-flex -->`), and still inside the `.container` div, add:

```html
      <p class="hero-tagline">Creative technologist building at the intersection of design and code. Full-stack dev, CG artist, and motion designer based in Singapore.</p>
      <div class="hero-skills">
        <span class="skill-label dev">DEV</span>
        <span class="skill-label art">ART</span>
        <span class="skill-label motion">MOTION</span>
      </div>
```

- [ ] **Step 4: Add dot-grid class to hero**

In `index.html` line 106, add the `dot-grid` class to the hero div:

```html
  <div data-w-id="87e28539-bfc6-1e4f-0808-65f3c938a801" class="hero dot-grid">
```

- [ ] **Step 5: Add gradient bar to `work.html`**

In `work.html`, immediately after `<body>` (line 36), before the skip-link, add:

```html
  <div class="gradient-bar" aria-hidden="true"></div>
```

- [ ] **Step 6: Preview and verify**

Open `http://localhost:8000` in a browser.
Expected: Thin crimson→teal gradient line at top of page. Hero is now warm light background with dark text. Tagline and DEV/ART/MOTION labels visible below the name. Dot grid texture visible on hero. Subtle geometric shapes in corners.

- [ ] **Step 7: Commit**

```bash
git add css/mingkey88-com.webflow.css index.html work.html
git commit -m "feat: light hero with gradient bar, tagline, and skill labels"
```

---

### Task 3: About / Experience / Achievements Section

**Files:**
- Modify: `css/mingkey88-com.webflow.css` (append to override block)
- Modify: `index.html:159` (add dot-grid class)
- Modify: `index.html:173,183,193,203` (add company-name class)

- [ ] **Step 1: Add about section CSS overrides**

Append to the override block in `css/mingkey88-com.webflow.css`:

```css
/* --- About / Experience section --- */
.section.gray {
  position: relative;
}

/* Geometric watermark on about section */
.section.gray::after {
  content: "";
  position: absolute;
  top: -40px;
  right: -30px;
  width: 200px;
  height: 200px;
  background: linear-gradient(135deg, #C0392B, #e74c3c);
  border-radius: 16px;
  transform: rotate(15deg);
  opacity: 0.04;
  pointer-events: none;
}

/* Bolder section headings */
.section.gray .page-title.flex.third {
  font-weight: 800;
  color: #1a1a1a;
  opacity: 1;
}

.section.gray .page-title.flex.second {
  font-weight: 800;
  color: #1a1a1a;
}

/* Crimson company names */
.company-name {
  color: #C0392B;
}

/* Subtler divider lines */
.line-divider-award .line-absolute {
  opacity: 0.4;
}
```

- [ ] **Step 2: Add dot-grid class to about section in `index.html`**

On line 159 of `index.html`, add the `dot-grid` class:

```html
  <section class="section gray dot-grid">
```

- [ ] **Step 3: Add `company-name` class to company name elements in `index.html`**

On these lines, add `company-name` as an additional class:

Line 173:
```html
<h3 id="w-node-a1b123f2-eaf6-98ec-0b5c-ee518e304063-6bd2f44a" class="display-2 second _1-4-rem company-name">Li-nk Limousine</h3>
```

Line 183:
```html
<h3 id="w-node-a5c9b3b3-ec45-7148-8612-f2d0f8e4f1b9-6bd2f44a" class="display-2 second _1-4-rem company-name">Great Eastern</h3>
```

Line 193:
```html
<h3 id="w-node-_252ece4f-c3ea-04c0-bcf9-5cea803d1d4a-6bd2f44a" class="display-2 second _1-4-rem company-name">RSAF</h3>
```

Line 203:
```html
<h3 id="w-node-_72262d78-23ca-1631-422d-2a5abd2f23fd-6bd2f44a" class="display-2 second _1-4-rem company-name">Freelance &amp; Productions</h3>
```

- [ ] **Step 4: Preview and verify**

Open `http://localhost:8000` in a browser and scroll to the about section.
Expected: Dot grid texture visible. Section headings ("Experience", "Achievements & Education") are bolder and darker. Company names (Li-nk Limousine, Great Eastern, RSAF, Freelance & Productions) are crimson. Subtle geometric watermark in top-right corner.

- [ ] **Step 5: Commit**

```bash
git add css/mingkey88-com.webflow.css index.html
git commit -m "feat: about section with dot grid, bold headings, crimson company names"
```

---

### Task 4: Project Cards Section — Dark to Light

**Files:**
- Modify: `css/mingkey88-com.webflow.css` (append to override block)
- Modify: `index.html:272` (add dot-grid class to work section)

- [ ] **Step 1: Add project section CSS overrides**

Append to the override block in `css/mingkey88-com.webflow.css`:

```css
/* --- Project cards section: dark → light --- */
.section:not(.gray):not(.with-work-page):not(.with-utility) {
  background-color: #FAFAFA;
  color: #1a1a1a;
}

.section:not(.gray) .page-title.flex {
  color: #1a1a1a;
  font-weight: 800;
}

.section:not(.gray) .page-title.flex .small {
  color: #999;
  font-weight: 400;
}

.project-photo._01 {
  border-radius: 8px;
}

.project-small-title.biger {
  color: #1a1a1a;
}

.project-small-title.lighter {
  color: #999;
}

/* Page divider visibility on light bg */
.page-divider {
  border-bottom: 1px solid rgba(0,0,0,0.06);
}
```

- [ ] **Step 2: Add dot-grid class to work section in `index.html`**

On line 272 of `index.html`:

```html
  <section class="section dot-grid">
```

- [ ] **Step 3: Preview and verify**

Open `http://localhost:8000` in a browser and scroll to the project cards.
Expected: Light background with dot grid. Project images have rounded corners. Titles are dark, categories are gray. Dividers are subtle light lines. The "Work ©14 — 25" heading is bold and dark.

- [ ] **Step 4: Commit**

```bash
git add css/mingkey88-com.webflow.css index.html
git commit -m "feat: project cards section switched to light background"
```

---

### Task 5: Skills Banner — Dark to Light

**Files:**
- Modify: `css/mingkey88-com.webflow.css` (append to override block)
- Modify: `index.html:464-485` (add dot-grid class, update separator markup)

- [ ] **Step 1: Add skills banner CSS overrides**

Append to the override block in `css/mingkey88-com.webflow.css`:

```css
/* --- Skills banner: dark → light --- */
.section-cta.for-services {
  background-color: #f0f0f0;
  position: relative;
}

.text-rotator-big.for-services {
  color: #1a1a1a;
}

/* Colored dot separators replace happy-face circles */
.happy-face-hero._130px {
  background-color: transparent;
  width: 12px;
  height: 12px;
  border-radius: 50%;
  margin-left: 30px;
  margin-right: 30px;
}

.text-rotator-content:first-child .happy-face-hero._130px {
  background-color: #C0392B;
}

.text-rotator-content:last-child .happy-face-hero._130px {
  background-color: #2A9D8F;
}
```

- [ ] **Step 2: Add dot-grid class to skills banner in `index.html`**

On line 464 of `index.html`:

```html
  <section data-w-id="052a9d9b-9b05-ca71-b6f6-08d4179d958b" class="section-cta for-services dot-grid">
```

- [ ] **Step 3: Preview and verify**

Open `http://localhost:8000` in a browser and scroll to the skills marquee.
Expected: Light gray background with dot grid. Bold dark text for skill names. Small crimson dots in the first row, teal dots in the second row. Scrolling animation still works.

- [ ] **Step 4: Commit**

```bash
git add css/mingkey88-com.webflow.css index.html
git commit -m "feat: skills banner switched to light with colored dot separators"
```

---

### Task 6: Footer & Profile Photo Section

**Files:**
- Modify: `css/mingkey88-com.webflow.css` (append to override block)
- Modify: `index.html:145` (add dot-grid to photo section)
- Modify: `index.html:487` (add dot-grid to footer)

- [ ] **Step 1: Add footer and photo section CSS overrides**

Append to the override block in `css/mingkey88-com.webflow.css`:

```css
/* --- Profile photo section --- */
.section-photo.video {
  position: relative;
  background-color: #FAFAFA;
}

/* --- Footer --- */
.footer-main {
  position: relative;
}

.footer-main .page-title.flex.second {
  font-weight: 800;
  color: #1a1a1a;
}

.footer-main .page-title.flex.third {
  text-transform: uppercase;
  letter-spacing: 1px;
  font-size: 11px;
  color: #999;
  opacity: 1;
}

/* Footer geometric watermark */
.footer-main::after {
  content: "";
  position: absolute;
  bottom: -60px;
  right: -30px;
  width: 200px;
  height: 200px;
  background: linear-gradient(135deg, #2A9D8F, #45b7aa);
  border-radius: 50%;
  opacity: 0.04;
  pointer-events: none;
}
```

- [ ] **Step 2: Add dot-grid class to profile photo section in `index.html`**

On line 145 of `index.html`:

```html
  <section class="section-photo video dot-grid">
```

- [ ] **Step 3: Add dot-grid class to footer in `index.html`**

On line 487 (the footer element — exact line may have shifted from earlier edits, find `<footer data-w-id=`):

```html
  <footer data-w-id="430377ad-3499-b0a3-857b-a4511ae5d3e9" class="footer-main dot-grid" role="contentinfo">
```

- [ ] **Step 4: Preview and verify**

Open `http://localhost:8000` in a browser.
Expected: Profile photo section has dot grid texture. Footer has dot grid, bold CTA heading, uppercase section labels ("NAVIGATION", "CONNECT", "CONTACT"), subtle teal watermark in bottom-right.

- [ ] **Step 5: Commit**

```bash
git add css/mingkey88-com.webflow.css index.html
git commit -m "feat: footer and profile section with dot grid and bold typography"
```

---

### Task 7: Work Page (`work.html`) Consistency

**Files:**
- Modify: `work.html` (add dot-grid classes to sections, gradient bar already added in Task 2)

- [ ] **Step 1: Check work.html section classes**

Read `work.html` to find the section and footer elements that need the `dot-grid` class.

- [ ] **Step 2: Add dot-grid class to work page sections**

Find the `.section.with-work-page` element and the footer in `work.html` and add `dot-grid` class to both:

```html
<!-- The work listing section -->
<section class="section with-work-page dot-grid">

<!-- The footer -->
<footer data-w-id="430377ad-3499-b0a3-857b-a4511ae5d3e9" class="footer-main dot-grid" role="contentinfo">
```

- [ ] **Step 3: Preview and verify**

Open `http://localhost:8000/work.html` in a browser.
Expected: Gradient bar at top. Work section has light background with dot grid (inherits from the CSS changes in Task 4). Footer matches index.html footer styling.

- [ ] **Step 4: Commit**

```bash
git add work.html
git commit -m "feat: work page consistent with visual refresh"
```

---

### Task 8: Responsive Breakpoint Checks & Cleanup

**Files:**
- Modify: `css/mingkey88-com.webflow.css` (append responsive overrides if needed)

- [ ] **Step 1: Check mobile breakpoints**

Open `http://localhost:8000` and resize to mobile width (< 480px). Check:
- Gradient bar still visible (3px, should be fine)
- Hero tagline doesn't overflow
- Skill labels wrap properly
- Dot grid not too visually dense on small screens
- Geometric watermarks don't clip awkwardly
- Skills banner dots are properly sized at mobile breakpoints

- [ ] **Step 2: Add responsive fixes if needed**

If any issues found, append responsive overrides inside the existing media queries or at the end of the override block:

```css
/* --- Responsive fixes for visual refresh --- */
@media screen and (max-width: 767px) {
  .hero-tagline {
    font-size: 0.9rem;
    max-width: 100%;
  }

  .hero::before {
    width: 150px;
    height: 150px;
  }

  .hero::after {
    width: 120px;
    height: 120px;
  }

  .section.gray::after {
    width: 120px;
    height: 120px;
  }
}

@media screen and (max-width: 479px) {
  .hero-skills {
    gap: 8px;
  }

  .hero-skills .skill-label {
    font-size: 10px;
    letter-spacing: 1px;
  }

  .happy-face-hero._130px {
    width: 8px;
    height: 8px;
    margin-left: 15px;
    margin-right: 15px;
  }
}
```

- [ ] **Step 3: Final full-page scroll test**

Scroll through entire page on both desktop and mobile widths. Verify:
- No dark background sections remain (hero, projects, skills all light)
- Dot grid is consistent across all sections
- Typography is consistently bold/dark
- Crimson/teal accents appear in: gradient bar, company names, skill labels, dot separators, watermarks
- Webflow scroll animations still fire
- Skills marquee still scrolls

- [ ] **Step 4: Commit**

```bash
git add css/mingkey88-com.webflow.css
git commit -m "fix: responsive adjustments for visual refresh"
```

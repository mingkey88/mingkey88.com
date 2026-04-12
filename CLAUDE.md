# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a static portfolio website for "Mingkey" (Ming Jie Tan) — a creative technologist based in Singapore. Originally exported from a Webflow template, it has been personalized with real resume content, a custom color scheme, and additional scroll animations.

## Architecture

**Static HTML Site** - No build process, no package manager, no server-side code.

- `index.html` - Home page with hero, profile photo, about, experience, achievements, work samples, skills banner, and footer
- `work.html` - Portfolio gallery page
- `detail_project.html` - Project detail template (CMS-based, currently empty placeholders)
- `401.html`, `404.html` - Error pages
- `css/` - Stylesheets
  - `normalize.css` - CSS reset
  - `webflow.css` - Webflow framework styles
  - `mingkey88-com.webflow.css` - Custom site styles (includes color theme and scroll animation CSS)
- `js/webflow.js` - Webflow interactions and animations runtime
- `images/` - All static assets including `mingjie-profile.jpg` (profile photo)
- `template-info/` - Template documentation (style-guide, licensing, changelog)

## Color Theme

The site uses a color palette derived from Ming Jie's resume:

- **Dark charcoal `#3D3D3D`** — hero background, nav, dark sections (CSS var: `--colors--first-color`, `--color--first-color`, `--colors--fourth-color`)
- **Crimson red `#C0392B`** — accent buttons, lines, highlights (CSS var: `--button--button-color`)
- **Light gray `#F5F5F5`** — secondary backgrounds (CSS var: `--colors--second-color`)
- **Off-white `#FAFAFA`** — body background (CSS var: `--color--second-color`)
- **Teal `#2A9D8F`** — social icon circles, accent highlights (hardcoded on `.social-circle` classes)

## Key Considerations

- **Webflow Export Base**: HTML/CSS originated from Webflow. Heavy use of Webflow's class naming conventions (e.g., `w-inline-block`, `w-dyn-list`, `w-dyn-bind-empty`).
- **CMS Placeholders**: Dynamic content areas use `w-dyn-*` classes with empty bindings. Project cards in the work section still use placeholder images and titles.
- **Webflow Interactions**: Original animations are powered by `webflow.js` using `data-w-id` attributes. There is an inline style override in `index.html` (`[data-w-id] { opacity: 1 !important; }`) that forces all animated elements visible outside Webflow hosting.
- **Custom Scroll Animations**: The about/experience/achievements section (section 3) uses CSS classes `scroll-reveal` and `scroll-delay-{1-5}` with an Intersection Observer script at the bottom of `index.html`. Elements fade up into view on scroll.
- **Profile Photo Section**: Section 2 uses the same `.project-wrapper-link` / `.project-photo` / border-part structure as the work cards, with custom sizing via `.section-photo.video .container` and `.section-photo.video .project-photo._01` overrides.
- **Fonts**: Google Fonts (DM Sans, Cormorant Garamond, Caveat, Caveat Brush) loaded via CSS `<link>` with `display=swap` for performance (replaced WebFont JS loader).
- **Contact Info**: Email links point to `mingjie.tan88@gmail.com`. Social links (LinkedIn, GitHub) in the hero and footer still use generic base URLs — need actual profile URLs.
- **No Build Step**: To preview, open HTML files directly in a browser or use any static file server.

## Lighthouse & Accessibility

The site has been optimized for maximum Lighthouse scores:

- **Semantic HTML**: `<header>`, `<nav>`, `<main id="main-content">`, `<footer role="contentinfo">` landmarks on both pages
- **Skip Link**: Hidden "Skip to main content" link appears on focus (keyboard navigation)
- **Single H1**: Only one `<h1>` per page; nav menu items use `<span>` instead of headings
- **ARIA**: Hamburger menu is a `<button>` with `aria-label` and `aria-expanded`; social links have `aria-label`; decorative images use `role="presentation"`
- **External Links**: All `target="_blank"` links include `rel="noopener noreferrer"`
- **Image Dimensions**: Explicit `width`/`height` on images to prevent CLS
- **Structured Data**: JSON-LD `Person` schema on `index.html`
- **SEO**: Open Graph + Twitter Card meta tags with `og:image`; unique meta descriptions per page; `theme-color` meta tag
- **Files**: `robots.txt` and `sitemap.xml` at project root

## Content (from resume)

- **Experience**: Li-nk Limousine (2024), Great Eastern Financial Advisor (2018–2024), RSAF Pilot Trainee (2017), Animator/Designer (2014–2017)
- **Education**: Full-Stack Web Dev with AI — Mages Institute (2025), Data Science & AI — IBM (2022), NUS Venture Building (2021), BFA Digital Art & Animation — SIT/DigiPen (2014)
- **Awards**: Best in Subject (Art) GCE A Level, WDA–DigiPen Scholarship

## Development

```bash
# Preview the site locally
python -m http.server 8000
# Then open http://localhost:8000
```

## GitHub

Repository: https://github.com/mingkey88/mingkey88.com

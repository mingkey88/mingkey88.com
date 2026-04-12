# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a static portfolio website exported from Webflow. It's a personal portfolio for "Mingkey" (Ming Jie Tan) showcasing creative development and design work.

## Architecture

**Static HTML Site** - No build process, no package manager, no server-side code.

- `index.html` - Home page with hero, about, experience, awards, and work sections
- `work.html` - Portfolio gallery page
- `detail_project.html` - Project detail template (CMS-based, currently empty placeholders)
- `401.html`, `404.html` - Error pages
- `css/` - Stylesheets
  - `normalize.css` - CSS reset
  - `webflow.css` - Webflow framework styles
  - `mingkey88-com.webflow.css` - Custom site styles
- `js/webflow.js` - Webflow interactions and animations runtime
- `images/` - All static assets
- `template-info/` - Template documentation (style-guide, licensing, changelog)

## Key Considerations

- **Webflow Export**: All HTML/CSS is Webflow-generated. Heavy use of Webflow's class naming conventions (e.g., `w-inline-block`, `w-dyn-list`, `w-dyn-bind-empty`).
- **CMS Placeholders**: Dynamic content areas use `w-dyn-*` classes with empty bindings. To add real projects, replace the placeholder elements or manually populate the content.
- **Interactions**: Animations are powered by `webflow.js` using `data-w-id` attributes. Modifying these requires understanding Webflow's interaction system. There is an inline style override in `index.html` (`[data-w-id] { opacity: 1 !important; }`) that forces all animated elements visible — this was added to work around animation visibility issues outside of Webflow hosting.
- **Fonts**: Google Fonts (DM Sans, Cormorant Garamond, Caveat, Caveat Brush) loaded via WebFont loader.
- **Template Placeholders**: Some content still uses template defaults (e.g., `contact@maia.studio` mailto link, generic social media URLs). These need to be replaced with real values.
- **No Build Step**: To preview, open HTML files directly in a browser or use any static file server (e.g., `python -m http.server`).

## Development

```bash
# Preview the site locally
python3 -m http.server 8000
# Then open http://localhost:8000
```

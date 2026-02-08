# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Academic portfolio website for Serena DeStefani, built with **Quarto** and deployed to **GitHub Pages** at `seredes.github.io`. The site uses the `cosmo` Bootstrap theme with custom SCSS overrides and the `trestles` about-page template.

## Build and Preview Commands

```bash
# Preview locally with hot-reload
quarto preview

# Render the full site (output goes to docs/)
quarto render

# Render a single page
quarto render about.qmd

# Preview a single lecture slide deck
quarto preview teaching/lecture1-slides.qmd
```

The output directory is `docs/`, which is served directly by GitHub Pages. The `.nojekyll` file in `docs/` prevents GitHub from running Jekyll processing.

## Architecture

- **`_quarto.yml`** -- Central config: navbar (left: page links with icons, right: social icon links), theme (`cosmo` + `styles.scss`), output directory (`docs/`), Font Awesome CDN include.
- **`.qmd` files** -- Each file is one page. Active pages: `index.qmd` (homepage/about), `about.qmd` (research), `teaching.qmd`, `cv.qmd`. Inactive pages still exist: `leadership.qmd`, `adventures.qmd`.
- **`index.qmd`** -- Uses Quarto's `about` template (`trestles`) with `#hero-heading` div for centered profile image + bio layout. Social links are in the navbar, not on this page.
- **`cv.qmd`** -- Embeds `DeStefani_CV.pdf` via an `<iframe>` with a download button. When updating the CV, replace the PDF file at the project root.
- **`styles.scss`** -- Custom SCSS overrides: transparent navbar styling, centered trestles layout, link colors, and lecture slide container styles.
- **`fontawesome.html`** -- Included in `<head>` via `_quarto.yml`; loads Font Awesome 6 from CDN for navbar icons.
- **`_extensions/quarto-ext/fontawesome/`** -- Font Awesome extension for shortcodes in page content (e.g., `{{< fa github >}}`).
- **`favicon.png`** -- Pixel art space invader with "SD" initials. Backup favicons stored in `images/`.
- **`docs/`** -- Generated output directory. Do not edit files here directly; they are overwritten by `quarto render`.
- **`.quarto/`** -- Build cache (gitignored). Safe to delete for a clean rebuild.

## Navbar Structure

The navbar uses inline HTML Font Awesome `<i>` tags (not shortcodes) for icons:

- **Left:** Home (`fa-house`), Research (`fa-flask`), Teaching (`fa-person-chalkboard`), CV (`fa-note-sticky`)
- **Right:** Icon-only social links -- Email (`fa-at`), LinkedIn, GitHub, X, Bluesky, Google Scholar

To add or edit navbar icons, use the format: `'<i class="fa-solid fa-icon-name fa-lg fa-fw"></i> Label'` for left items, or icon-only (no label text) for right items.

## Teaching Slides Architecture

Lecture slides use **Quarto Reveal.js** and follow a two-file pattern per lecture:

- **`teaching/lectureN-slides.qmd`** -- The Reveal.js slide deck (standalone presentation)
- **`teaching/lectureN.qmd`** -- A detail page that embeds the slides via `<iframe>` with a full-screen link
- **`teaching/lectureN_images/`** -- Extracted images from the original PowerPoint, named `slide{N}_img{M}.{ext}`
- **`teaching/slides-style.css`** -- Shared CSS for all slide decks (caps images at `max-height: 32vh`, handles column overflow)

The full workflow for converting PowerPoint to Quarto Reveal.js is documented in `teaching/README.md`, including image extraction (via `python-pptx`), YAML header template, slide syntax patterns (two-column layouts, incremental reveals, section dividers), and common image troubleshooting.

## Deployment

Push to the `main` branch on GitHub. GitHub Pages is configured to serve from the `docs/` directory. Always run `quarto render` before committing so that `docs/` contains the latest output.

## Key Conventions

- The visual editor mode is enabled (`editor: visual` in `_quarto.yml`), but `.qmd` source files can be edited as plain markdown.
- Navbar left items are defined in `_quarto.yml` under `website.navbar.left`, right social icons under `website.navbar.right`. To add a new page, create a `.qmd` file and add a corresponding navbar entry with an icon.
- Images go in the `images/` directory (site-wide) or `teaching/lectureN_images/` (lecture-specific).
- The project includes an `.Rproj` file for RStudio integration; R code chunks can be used in `.qmd` files if needed.
- Font Awesome in navbar: inline HTML `<i>` tags (loaded via CDN). Font Awesome in page content: shortcodes `{{< fa icon-name >}}` (loaded via extension).
- Use `--` (double dash) instead of em dashes in text.

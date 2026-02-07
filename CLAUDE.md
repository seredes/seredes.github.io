# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Academic portfolio website for Serena DeStefani, built with **Quarto** and deployed to **GitHub Pages** at `seredes.github.io`. The site uses the `cosmo` Bootstrap theme with the `trestles` about-page template.

## Build and Preview Commands

```bash
# Preview locally with hot-reload
quarto preview

# Render the full site (output goes to docs/)
quarto render

# Render a single page
quarto render about.qmd
```

The output directory is `docs/`, which is served directly by GitHub Pages. The `.nojekyll` file in `docs/` prevents GitHub from running Jekyll processing.

## Architecture

- **`_quarto.yml`** -- Central config: defines site title, navbar links, theme (`cosmo`), output directory (`docs/`), and global format options (TOC enabled, custom CSS).
- **`.qmd` files** -- Each file is one page. Uses Quarto Markdown with YAML front matter. Pages: `index.qmd` (homepage/about), `about.qmd` (research), `teaching.qmd`, `leadership.qmd`, `adventures.qmd`, `cv.qmd`.
- **`index.qmd`** -- Uses Quarto's `about` template (`trestles`) with `#hero-heading` div for profile layout and icon links.
- **`cv.qmd`** -- Embeds `DeStefani_CV.pdf` via an `<iframe>` with a download button. When updating the CV, replace the PDF file at the project root.
- **`styles.css`** -- Custom CSS overrides (currently minimal).
- **`_extensions/quarto-ext/fontawesome/`** -- Font Awesome extension for icon shortcodes.
- **`docs/`** -- Generated output directory. Do not edit files here directly; they are overwritten by `quarto render`.
- **`.quarto/`** -- Build cache (gitignored). Safe to delete for a clean rebuild.

## Deployment

Push to the `main` branch on GitHub. GitHub Pages is configured to serve from the `docs/` directory. After making changes, run `quarto render` before committing so that `docs/` contains the latest output.

## Key Conventions

- The visual editor mode is enabled (`editor: visual` in `_quarto.yml`), but `.qmd` source files can be edited as plain markdown.
- Navigation is defined in `_quarto.yml` under `website.navbar.left`. To add a new page, create a `.qmd` file and add a corresponding navbar entry.
- Images go in the `images/` directory.
- The project includes an `.Rproj` file for RStudio integration; R code chunks can be used in `.qmd` files if needed.

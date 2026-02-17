# Style Revert Notes

## Navbar: Home icon

The original Home navbar entry was:

```yaml
- text: '<i class="fa-solid fa-house fa-lg fa-fw"></i> Home'
  href: index.qmd
```

To revert from "About" back to "Home", replace the About entry in `_quarto.yml` with the line above.

---

## Navbar: About tab (removed)

The About tab was removed because it was redundant with the site title "Serena DeStefani" (both link to `index.qmd`). The original entry was:

```yaml
- text: '<i class="fa-solid fa-circle-user fa-lg fa-fw"></i> About'
  href: index.qmd
```

To restore it, add the entry back to `_quarto.yml` under `website.navbar.left` (it was the first item).

---

## Teaching: Lecture 1 embedded slides (moved to course site)

Lecture 1 was embedded on the personal site using a two-file pattern. It has been moved to the course site at `quant-methods-psych/content/01-content.qmd`. To restore it on the personal site:

**Detail page** (`teaching/lecture1.qmd`):

```yaml
---
title: "Lecture 1: Introduction"
subtitle: "Quantitative Methods in Psychology"
date: "7/6/2020"
toc: false
body-classes: lecture-detail
---
```

Body content used a description paragraph, then a "Open slides in full screen" link and an iframe:

```markdown
::: {.slide-link-bar}
[{{< fa arrow-up-right-from-square >}} Open slides in full screen](lecture1-slides.html){target="_blank"}
:::

::: {.slide-container}
<iframe class="slide-frame" src="lecture1-slides.html"></iframe>
:::
```

**Slide deck** (`teaching/lecture1-slides.qmd`): Quarto Reveal.js format with `css: slides-style.css`, width 1050, height 700.

**CSS classes** used: `.lecture-detail`, `.slide-link-bar`, `.slide-container`, `.slide-frame` (defined in `styles.scss`).

The teaching page (`teaching.qmd`) linked to it via `[Lecture 1: Introduction](teaching/lecture1.html)`.

---

# Font Options for This Site

## Current fonts (system defaults)

Body and headings both use the system font stack:

```scss
$font-family-sans-serif: system-ui, -apple-system, sans-serif;
```

No Google Fonts import is needed. No `$headings-font-family` variable is set.

## Alternative: Jost + Libre Franklin (Google Fonts)

These are the fonts used on Andrew Heiss's site. To try them again:

1. In `styles.scss`, under `/*-- scss:defaults --*/`, set:

```scss
$font-family-sans-serif: "Libre Franklin" !default;
$headings-font-family: "Jost" !default;
$headings-font-weight: 600 !default;

$navbar-font-family: "Jost" !default;
$toc-font-family: "Jost" !default;
$footer-font-family: "Jost" !default;
```

2. In `styles.scss`, under `/*-- scss:rules --*/`, add the Google Fonts import **before** any other rules:

```scss
$web-font-path: "https://fonts.googleapis.com/css2?family=Jost:ital,wght@0,100..900;1,100..900&family=Libre+Franklin:ital,wght@0,100..900;1,100..900&display=swap" !default;

@if $web-font-path {
    @import url($web-font-path);
}
```

3. To revert back to system fonts, remove all of the above and set `$font-family-sans-serif: system-ui, -apple-system, sans-serif;` instead.

---

## Favicon: Heiss hexagon (replaced space invader)

**Old favicon:** Pixel art orange space invader with "SD" initials (`images/favicon-space-invader-sd.png`).

**New favicon:** Andrew Heiss's geometric hexagon with red/coral triangular segments and dark navy borders. Copied from his site repo (`ath-quarto/files/favicon-512.png`).

**To revert to the original space invader favicon**, run:

```bash
cp images/favicon-space-invader-sd.png favicon.png
```

Then re-render the site with `quarto render`. The `_quarto.yml` entry (`favicon: favicon.png`) does not need to change -- it uses the same filename either way.

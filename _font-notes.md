# Style Revert Notes

## Navbar: Home icon

The original Home navbar entry was:

```yaml
- text: '<i class="fa-solid fa-house fa-lg fa-fw"></i> Home'
  href: index.qmd
```

To revert from "About" back to "Home", replace the About entry in `_quarto.yml` with the line above.

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

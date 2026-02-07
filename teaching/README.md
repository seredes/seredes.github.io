# Teaching Slides Workflow

Guide for converting PowerPoint lectures to Quarto Reveal.js presentations.

## Quick Start

```bash
# Preview with hot-reload
quarto preview lecture1.qmd

# Render to HTML
quarto render lecture1.qmd
```

Output goes to `../docs/teaching/`.

## Converting a New PowerPoint

### 1. Extract images

Use `python-pptx` to extract all images from the `.pptx` file into a folder named `<lecture>_images/`:

```python
from pptx import Presentation
import os

prs = Presentation('LectureX.pptx')
outdir = 'lectureX_images'
os.makedirs(outdir, exist_ok=True)

for slide_idx, slide in enumerate(prs.slides):
    slide_num = slide_idx + 1
    img_count = 0
    for shape in slide.shapes:
        if shape.shape_type == 13:  # Picture
            img_count += 1
            ext = shape.image.content_type.split('/')[-1]
            if ext == 'x-wmf': ext = 'wmf'
            elif ext == 'jpeg': ext = 'jpg'
            fname = f"slide{slide_num}_img{img_count}.{ext}"
            with open(os.path.join(outdir, fname), 'wb') as f:
                f.write(shape.image.blob)
```

**Note:** WMF files (Windows Metafile) cannot be converted on macOS without LibreOffice or ImageMagick. They are typically decorative backgrounds and can be skipped.

### 2. Create the .qmd file

Use this YAML header:

```yaml
---
title: "Lecture Title"
subtitle: "Course Name"
author: "Serena DeStefani"
date: "MM/DD/YYYY"
format:
  revealjs:
    theme: default
    slide-number: c
    transition: slide
    width: 1050
    height: 700
    css: slides-style.css
    menu:
      numbers: true
---
```

Key settings:
- `slide-number: c` -- shows current slide number only (not "current/total")
- `menu: numbers: true` -- shows slide numbers in the outline menu (press `M` to open)
- `slides-style.css` -- shared CSS for image sizing (see below)

### 3. Slide syntax

**Basic slide:**
```markdown
## Slide Title

Content here.
```

**Two-column layout (text + image):**
```markdown
## Slide Title

:::: {.columns}
::: {.column width="55%"}
Text content here.
:::
::: {.column width="45%"}
![](lectureX_images/slideN_img1.png){fig-align="center" width="90%"}
:::
::::
```

**Incremental reveal (content appears on click):**
```markdown
## Slide Title

First point visible immediately.

. . .

This appears on click.

. . .

This appears on next click.
```

**Section divider slide:**
```markdown
## {.center}

### Chapter Title

#### Subtitle
```

**Smaller text (for dense slides):**
```markdown
## Slide Title {.smaller}
```

**Images without captions:**
Use empty alt text `![]()` to prevent captions from showing under images.

### 4. Common image issues

Images overflowing the slide is the most common problem. The shared `slides-style.css` caps images at `max-height: 32vh`. Additional fixes:

- **Two images stacked vertically:** Place them side by side in columns instead
- **Blurry images:** Small images from PowerPoint look blurry when scaled up. Recreate simple content (equations, tables) using LaTeX math (`$$...$$`) or markdown tables
- **Image too close to edges:** Reduce the `width` percentage on the image

### 5. Feedback loop for fixing slides

1. Open the presentation and press `M` to open the outline menu
2. Note the slide number shown in the outline
3. Report the number and the issue (e.g., "slide 37: image overflows")
4. After fixing and re-rendering, navigate using the outline menu

## Presenting

- **Speaker View:** Press `S` to open a presenter window with current slide, next slide preview, speaker notes, and a timer
- **Add speaker notes** to any slide:
  ```markdown
  ## Slide Title

  Content...

  ::: {.notes}
  Speaker notes go here. Only visible in speaker view.
  :::
  ```
- **Fullscreen:** Press `F`
- **Overview/grid:** Press `O`
- **Keyboard help:** Press `?`

## Shared Files

- `slides-style.css` -- shared CSS for all lecture slides (image max-height, margins, column overflow)
- `README.md` -- this file

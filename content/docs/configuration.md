---
layout: docs
title: "Configuration"
section: "Reference"
order: 1
description: "Complete config.yaml reference for site metadata, themes, fonts, syntax highlighting, Markdown, navigation, and feeds."
---

All site-wide settings live in `config.yaml` at the root of your project. Every key in `config.yaml` is also available inside Jinja2 templates through the `site` object (for example `{{ site.site_name }}`).

## Site metadata

```yaml
site_name: "My Site"
author: "Your Name"
base_url: "https://example.com"
```

`base_url` is used to build absolute URLs in `sitemap.xml`, Atom/RSS feeds, and OpenGraph tags.

## Themes (DaisyUI v5)

`mettu` reads the `theme` block and generates `assets/css/generated.daisyui.css`:

```yaml
theme:
  default: "dim"
  include:
    - "dim"
    - "light"
    - "midnight"
  custom:
    midnight: |
      prefers-dark: true;
      color-scheme: dark;
      --color-base-100: #11131a;
      --color-base-200: #1b1f2a;
      --color-base-300: #272d3d;
      --color-base-content: #e6e8f0;
      --color-primary: #8aa2ff;
      --color-primary-content: #11131a;
```

- `default`: Initial DaisyUI theme applied to `<html data-theme="...">`.
- `include`: List of themes to bundle. If you include more than one theme, the built-in navbar shows a theme switcher that saves the visitor's preference in `localStorage`.
- `custom`: Map of custom theme names to raw DaisyUI v5 CSS variable blocks.

## Fonts and syntax highlighting

```yaml
fonts:
  imports:
    - "https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&family=Space+Grotesk:wght@600;700&family=JetBrains+Mono:wght@400;500&display=swap"
  families:
    body: "'Inter', sans-serif"
    heading: "'Space Grotesk', sans-serif"
    mono: "'JetBrains Mono', monospace"

syntax:
  pygments_theme: "native"
```

`fonts` generates `assets/css/generated.fonts.css`, and `syntax.pygments_theme` generates `assets/css/syntax.css` using any built-in Pygments style (such as `native`, `dracula`, `monokai`, `github-dark`, or `friendly`).

## Default layouts and Markdown extensions

```yaml
default_layout: "page"
default_layouts:
  blog: "post"
  til: "til"
  docs: "docs"

markdown:
  extensions:
    - fenced_code
    - codehilite
    - footnotes
    - tables
    - attr_list
    - sane_lists
    - md_in_html
    - toc:
        permalink: true
```

## Navigation

```yaml
navigation:
  - name: "Home"
    url: "/"
  - name: "Blog"
    url: "/blog/"
  - name: "Docs"
    url: "/docs/"
```

## Images

```yaml
images:
  formats:
    - webp
    - jpg
```

Supported formats are `webp`, `avif`, `jpg`, `jpeg`, and `png`. If omitted, `mettu` defaults to `["webp"]`.

## Feeds

```yaml
feeds:
  - filename: "feed.xml"
    layouts: ["post", "til"]
    title: "My Site"
    subtitle: "Latest posts and notes"
    content: false
  - filename: "blog/feed.xml"
    layouts: ["post"]
    title: "My Site - Blog"
    subtitle: "Long-form posts"
    content: true
```

Set `content: true` if you want full HTML bodies inside feed entries instead of short descriptions.

## Custom Python executable

If you need `mettu` to use a specific Python binary instead of auto-detecting `.venv` or `python3`, set `PY_EXECUTABLE` in your environment or add a `runtime` block:

```yaml
runtime:
  python_executable: "/usr/bin/python3.12"
```

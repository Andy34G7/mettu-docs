---
layout: docs
title: "Configuration"
section: "Reference"
order: 1
description: "Complete reference for config.yaml: themes, fonts, syntax highlighting, Markdown, navigation, and feeds."
---

All site-wide settings live in `config.yaml` at the root of your project.

## Site metadata

```yaml
site_name: "My Site"
author: "Your Name"
base_url: "https://example.com"
```

## Themes (DaisyUI v5)

`mettu` generates `assets/css/generated.daisyui.css` from `config.yaml`:

```yaml
theme:
  default: "dim"
  include:
    - "dim"
    - "light"
    - "mycustom"
  custom:
    mycustom: |
      prefers-dark: true;
      color-scheme: dark;
      --color-base-100: #1C1C3F;
      --color-primary: #A0A0FF;
```

When multiple themes are listed in `include`, the built-in navbar renders a theme switcher that persists the user's choice in `localStorage`.

## Fonts & syntax highlighting

```yaml
fonts:
  imports:
    - "https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&display=swap"
  families:
    body: "'Inter', sans-serif"
    heading: "'Space Grotesk', sans-serif"
    mono: "'JetBrains Mono', monospace"

syntax:
  pygments_theme: "native"
```

Pygments CSS is written to `assets/css/syntax.css` automatically.

## Markdown & default layouts

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
    - nl2br
    - toc:
        permalink: true
```

## Navigation & feeds

```yaml
navigation:
  - name: "Home"
    url: "/"
  - name: "Blog"
    url: "/blog/"
  - name: "Docs"
    url: "/docs/"

feeds:
  - filename: "feed.xml"
    layouts: ["post", "til"]
    title: "My Site"
    subtitle: "Latest posts and notes"
    content: false
```

If `feeds` is omitted, `mettu` generates default Atom/RSS feeds for `feed.xml`, `blog/feed.xml`, and `til/feed.xml`.

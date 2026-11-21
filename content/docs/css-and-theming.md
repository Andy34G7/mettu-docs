---
layout: docs
title: "CSS & Theming"
section: "Features"
order: 1
description: "Tailwind CSS v4, DaisyUI v5, generated stylesheets, custom themes, and per-page theme overrides."
---

`mettu` uses **Tailwind CSS v4** (via `@tailwindcss/vite`), **@tailwindcss/typography**, and **DaisyUI v5** so you can build clean UI components without writing hundreds of lines of custom CSS.

## How CSS is wired up

Your primary stylesheet is `assets/css/main.css`:

```css
@import "./generated.fonts.css";
@import "tailwindcss";
@plugin "@tailwindcss/typography";
@import "./generated.daisyui.css";
```

When you run `mettu dev` or `mettu build`, Python reads `config.yaml` and writes three generated files into `assets/css/`:

1. `generated.fonts.css`: `@import` rules for your web fonts plus CSS rules wiring `body`, headings, and `code`/`pre` to your configured font families.
2. `generated.daisyui.css`: `@plugin "daisyui"` configuration with your `theme.include` list and any custom theme blocks from `theme.custom`.
3. `syntax.css`: Pygments color classes matching `syntax.pygments_theme`.

You can add any custom CSS right below the four imports in `assets/css/main.css` (or import additional stylesheets like `@import "./custom.css";`), and Vite will bundle and minify everything together into `dist/assets/`.

## Custom DaisyUI themes

If built-in DaisyUI themes are not quite what you want, define a custom theme in `config.yaml` and list it in `theme.include`:

```yaml
theme:
  default: "paper"
  include:
    - "paper"
    - "dim"
  custom:
    paper: |
      prefers-dark: false;
      color-scheme: light;
      --color-base-100: #faf8f5;
      --color-base-200: #f0ece4;
      --color-base-300: #e2dcd0;
      --color-base-content: #1e1b18;
      --color-primary: #2d5a88;
      --color-primary-content: #ffffff;
```

## Per-page themes

You can override the theme on a specific page by setting `theme:` in its frontmatter:

```yaml
---
layout: page
title: "Poetry"
theme: "rosepine"
---
```

Make sure that theme name is also listed in `theme.include` in `config.yaml` so DaisyUI includes its variables in the compiled CSS.

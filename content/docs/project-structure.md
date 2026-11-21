---
layout: docs
title: "Project Structure"
section: "Overview"
order: 3
description: "What every directory in a mettu site does, what is generated at build time, and what to gitignore."
---

A `mettu` site keeps your authored files separate from generated build output and caches. Here is what a typical repository looks like:

```text
my-site/
├── config.yaml                  # Site metadata, themes, fonts, navigation, feeds
├── package.json                 # Depends on @andy34g7/mettu and defines scripts
├── assets/
│   ├── css/
│   │   ├── main.css             # Your main Tailwind v4 + DaisyUI entrypoint
│   │   ├── generated.daisyui.css  (generated)
│   │   ├── generated.fonts.css    (generated)
│   │   └── syntax.css             (generated)
│   ├── images/                  # Source images (processed by Sharp)
│   └── images-processed/          (generated)
├── content/                     # All your Markdown pages
│   ├── index.md                 # Renders to /index.html
│   ├── blog/
│   │   └── first-post.md        # Renders to /blog/first-post/index.html
│   └── docs/
│       └── index.md             # Renders to /docs/index.html
├── templates/                   # Optional Jinja2 overrides and custom layouts
├── public/                      # Static files copied straight to dist/ (CNAME, favicon.svg, robots.txt)
├── .cache/                        (generated: parse cache, image manifest, pip hash)
├── .venv/                         (generated: Python virtual environment)
└── dist/                          (generated: final production build)
```

## What belongs in git vs. `.gitignore`

Commit your authored files: `config.yaml`, `package.json`, `content/`, `assets/css/main.css` (and any custom CSS/JS/images), `public/`, `templates/` (if you have overrides), and `.github/workflows/deploy.yml`.

Ignore generated files in `.gitignore`:

```gitignore
node_modules/
dist/
.venv/
.cache/
.env
package-lock.json
assets/images-processed/
assets/css/generated.daisyui.css
assets/css/generated.fonts.css
assets/css/syntax.css
```

## How temporary files work during builds

One important detail to know about `mettu`: Python renders HTML files into the project root (for example `./index.html`, `./blog/index.html`, `./sitemap.xml`) so Vite can discover and bundle them as entrypoints into `dist/`.

When `mettu build` finishes, `mettu clean` automatically removes those temporary root files while preserving all standard project directories (`assets`, `bin`, `content`, `dist`, `node_modules`, `public`, `scripts`, `src`, `templates`, `tests`, `.cache`, `.git`, `.github`, `.venv`).

> [!WARNING]
> Avoid creating top-level custom directories at the project root that are not in `mettu`'s preserved list, because `mettu clean` treats unknown root directories as generated output. Keep static files in `public/` and assets in `assets/`.

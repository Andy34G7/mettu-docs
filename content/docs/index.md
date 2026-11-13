---
layout: docs
title: "Preface"
section: "Overview"
order: 1
description: "What mettu is, why it exists, and how its hybrid Python + Vite pipeline works."
---

## What is mettu?

`mettu` is a static site generator designed for personal sites, blogs, and documentation portals that want **simple Markdown authoring** without giving up **modern frontend tooling**.

Instead of reimplementing CSS bundlers in Python or writing a Markdown/template ecosystem from scratch in Node, `mettu` splits the job cleanly:

1. **Python (`src/main.py`)** handles content: parsing YAML frontmatter, rendering Markdown with Pygments syntax highlighting, evaluating Jinja2 templates, and generating `sitemap.xml`, `feed.xml`, tag pages, and `search-index.json`.
2. **Vite (`vite.config.mjs`)** handles assets: preprocessing responsive images with Sharp, compiling Tailwind CSS v4 + DaisyUI v5, bundling client scripts, and serving hot-reloaded previews during development.

## How a build flows

When you run `mettu build`:

1. `vite.config.mjs` ensures a local Python `.venv` exists and installs `mettu`'s Python dependencies if `requirements.txt` changed.
2. `src/image-preprocess.mjs` scans `assets/images/`, generates responsive formats (WebP/AVIF/JPEG), and writes `.cache/image-manifest.json` (optionally syncing to an S3-compatible bucket).
3. `src/main.py` generates `generated.daisyui.css`, `generated.fonts.css`, and `syntax.css`, parses `content/**/*.md` (using `.cache/page-cache.json` to skip unchanged files), and renders HTML into the working directory.
4. Vite bundles all HTML entrypoints, CSS, JS, and processed images into `dist/`, copies XML feeds/sitemaps and `search-index.json`, and cleans temporary HTML out of your root directory.

> [!TIP]
> Because `mettu` ships as an npm package (`@andy34g7/mettu`), your site repository only needs `config.yaml`, `content/`, `assets/`, `public/`, and any templates you choose to override.

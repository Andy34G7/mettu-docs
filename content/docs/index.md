---
layout: docs
title: "Preface"
section: "Overview"
order: 1
description: "What mettu is, why it exists, and how its hybrid Python and Vite build pipeline works."
---

## Why static site generators?

Most personal websites, blogs, and documentation sites do not need to run server code on every request. Once a page is written, the HTML barely changes until you edit it again.

Still, writing raw HTML by hand gets painful fast. You usually want:

- A shared navbar, footer, and page layout across every post.
- Automatic index pages (like a `/blog/` list sorted by date, or tag pages).
- Syntax highlighting, responsive images, RSS feeds, and a sitemap without manually maintaining them.
- Clean CSS styling without fighting build scripts for hours.

A static site generator (SSG) takes your Markdown files and templates at build time and outputs plain HTML, CSS, and JS that you can host anywhere for free.

## What does mettu do?

`mettu` started as a personal SSG built to make authoring fast and live previews painless. Instead of forcing one language to do everything, it splits the work between two tools that are already great at their jobs:

- **Python (`src/main.py`)** handles content: parsing YAML frontmatter, converting Markdown to HTML, highlighting code with Pygments, rendering Jinja2 templates, and generating tag pages, `sitemap.xml`, Atom/RSS feeds, and `search-index.json`.
- **Vite (`vite.config.mjs`)** handles assets and dev tooling: preprocessing responsive images with Sharp, compiling Tailwind CSS v4 and DaisyUI v5, bundling client scripts, and hot-reloading pages as you edit.

## How a build actually runs

When you run `npm run build` (which calls `mettu build`), four steps happen in sequence:

1. **Python environment check**: `mettu` checks for a local `.venv/` in your site root. If it does not exist (or `requirements.txt` changed), it creates `.venv/` and installs its Python dependencies once, then caches a hash in `.cache/requirements.hash` so future builds skip `pip`.
2. **Image preprocessing**: `src/image-preprocess.mjs` scans `assets/images/`, generates resized formats (WebP, AVIF, JPEG, etc.) with Sharp, and writes `.cache/image-manifest.json`. If S3 credentials are configured, it uploads them there instead of bundling them locally.
3. **Static page generation**: `src/main.py` generates `generated.daisyui.css`, `generated.fonts.css`, and `syntax.css` from `config.yaml`, parses `content/**/*.md` (using `.cache/page-cache.json` to skip unchanged Markdown files), and renders your HTML, feeds, sitemap, and search index.
4. **Vite bundle and cleanup**: Vite bundles all generated HTML entrypoints, CSS, JS, and processed images into `dist/`, copies XML feeds/sitemaps and `search-index.json` into `dist/`, and cleans the temporary HTML files out of your project root.

> [!TIP]
> Because `@andy34g7/mettu` ships as an npm package, you do not have to fork or copy engine files into your site repo. Your repo stays focused on content, configuration, and any templates you want to override.

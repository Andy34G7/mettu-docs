---
layout: docs
title: "Assets & Images"
section: "Features"
order: 1
description: "Static files, CSS entrypoints, responsive Sharp image preprocessing, and S3/R2 uploads."
---

## Static files (`public/`)

Anything placed in `public/` (such as `CNAME`, `robots.txt`, `favicon.svg`, or PDFs) is copied verbatim to the root of `dist/` by Vite.

## Stylesheets (`assets/css/`)

`assets/css/main.css` is your primary stylesheet entrypoint. During builds, `mettu` generates:

- `assets/css/generated.daisyui.css`
- `assets/css/generated.fonts.css`
- `assets/css/syntax.css`

All three are gitignored and regenerated from `config.yaml`.

## Responsive image preprocessing

Drop source images into `assets/images/`. Before Python renders your Markdown, `src/image-preprocess.mjs` runs Sharp to generate responsive breakpoints and formats configured in `config.yaml`:

```yaml
images:
  formats:
    - webp
    - jpg
```

When Python renders HTML, any `<img>` pointing to an image in `assets/images/` is automatically rewritten into a `<picture>` element with `<source srcset="...">` entries using `.cache/image-manifest.json`.

## Optional S3 / Backblaze B2 upload

If S3 credentials are present in `.env` (or environment variables), `mettu` uploads processed images to your bucket and rewrites `srcset` URLs to point to your remote bucket/CDN instead of bundling them into `dist/`.

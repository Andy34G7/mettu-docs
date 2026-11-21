---
layout: docs
title: "Assets & Images"
section: "Features"
order: 2
description: "Static files in public/, responsive Sharp image preprocessing, and optional S3 or Backblaze B2 uploads."
---

## Static files (`public/`)

Anything inside `public/` is copied straight to the root of `dist/` without modification. Put files like `CNAME`, `robots.txt`, `favicon.svg`, `pretty-feed-v3.xsl`, or downloadable PDFs here.

## Responsive image preprocessing

Drop source images (`.png`, `.jpg`, `.jpeg`, `.gif`) into `assets/images/`. You can organize them in subdirectories if you like.

Before Python renders your Markdown, `src/image-preprocess.mjs` processes each image with Sharp:

- Generates resized versions at **640px**, **1280px**, and the **original width** (never upscaling past the original width).
- Encodes each width into every format listed under `images.formats` in `config.yaml` (defaults to `["webp"]`; supports `webp`, `avif`, `jpg`, `jpeg`, `png`).
- Caches file mtime, size, and dimensions in `.cache/image-meta.json` so unchanged images are not re-decoded on every build.
- Writes `.cache/image-manifest.json` mapping each original filename to its generated variants.

During HTML rendering, Python scans `<img>` tags that reference `assets/images/...` and automatically rewrites them into responsive `<picture>` elements with `<source srcset="...">` entries.

In Markdown, just write a normal image link:

```markdown
![Architecture diagram](/assets/images/diagram.png)
```

## Optional S3 / Backblaze B2 / R2 uploads

If you have lots of high-resolution photos and do not want to bloat your git repo or `dist/` bundle, `mettu` can upload processed images to any S3-compatible bucket and point `srcset` URLs directly to your bucket.

Set these environment variables in `.env` (or in your CI secrets):

```dotenv
S3_BUCKET=your-bucket-name
S3_ENDPOINT=https://s3.us-east-1.amazonaws.com
S3_REGION=us-east-1
S3_ACCESS_KEY=your-access-key
S3_SECRET_KEY=your-secret-key
```

Before uploading each variant, `mettu` sends a `HeadObject` check so already-uploaded files are skipped. For self-hosted MinIO instances with self-signed certificates, you can also set `S3_INSECURE_TLS=true`.

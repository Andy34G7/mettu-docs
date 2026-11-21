---
layout: docs
title: "CLI & Deployment"
section: "Features"
order: 4
description: "All mettu CLI commands, upgrading @andy34g7/mettu, GitHub Pages Actions, and custom domains."
---

## CLI commands

| Command | Script | What it does |
| :--- | :--- | :--- |
| `mettu dev` | `npm run dev` | Starts Vite with file watchers, incremental Python rebuilds, and HMR. |
| `mettu build` | `npm run build` | Runs image preprocessing, full Python render, Vite production bundle into `dist/`, and root cleanup. |
| `mettu preview` | `npm run preview` | Serves the built `dist/` folder locally so you can test production output. |
| `mettu new "Title"` | `npm run new -- "Title"` | Scaffolds a new Markdown file (`--kind post`, `--kind page`, or `--kind doc`). |
| `mettu check` | `npm run check` | Validates `config.yaml`, templates, duplicate URLs, and broken links/images. |
| `mettu clean` | `npm run clean` | Removes temporary generated HTML files from the project root. |

## Upgrading mettu

Because your site consumes `@andy34g7/mettu` as a normal npm dependency, pulling in bug fixes or new features from `mettu` is a single command:

```bash
npm update @andy34g7/mettu
npm run check
npm run build
```

No git merges across repositories, and your `content/`, `config.yaml`, and custom `templates/` stay untouched.

## Deploying to GitHub Pages

Add `.github/workflows/deploy.yml`:

```yaml
name: Deploy to GitHub Pages

on:
  push:
    branches: [ main ]
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: pages
  cancel-in-progress: true

jobs:
  build:
    runs-on: ubuntu-latest
    environment: github-pages
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '20'
      - uses: actions/setup-python@v5
        with:
          python-version: '3.x'
      - run: npm install
      - run: npm run build
      - uses: actions/upload-pages-artifact@v3
        with:
          path: dist

  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    needs: build
    steps:
      - id: deployment
        uses: actions/deploy-pages@v4
```

Then in your repository's **Settings -> Pages**, set **Source** to **GitHub Actions**.

## Custom domains on GitHub Pages

If you are deploying a project repository to a subdomain (for example `mettu-docs.hemanthandey.dev`):

1. Put the domain name in `public/CNAME`.
2. In your DNS provider, create a `CNAME` record pointing the subdomain (e.g. `mettu-docs`) to `<your-github-username>.github.io`.
3. In **Settings -> Pages**, enter your custom domain and enable **Enforce HTTPS** once GitHub finishes provisioning the TLS certificate.

> [!NOTE]
> If you open a project repo at `<username>.github.io/<repo-name>/` before your custom domain is verified, root-relative asset paths like `/assets/...` will 404 because of the subpath. Once the custom domain is active and serving from `/`, all CSS and assets load normally.

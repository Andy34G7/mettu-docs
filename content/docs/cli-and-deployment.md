---
layout: docs
title: "CLI & Deployment"
section: "Features"
order: 2
description: "CLI commands for authoring, validating, building, and deploying a mettu site to GitHub Pages."
---

## CLI commands

| Command | Description |
| :--- | :--- |
| `mettu dev` | Start the Vite dev server with Python incremental rebuilds and HMR. |
| `mettu build` | Run a full production build into `dist/`. |
| `mettu preview` | Preview the production `dist/` directory locally. |
| `mettu new "Title"` | Scaffold a new Markdown file (`--kind post\|page\|doc`). |
| `mettu check` | Validate `config.yaml`, templates, duplicate URLs, and broken links. |
| `mettu clean` | Remove temporary generated HTML files from the project root. |

## Validating before deploying

Run `npm run check` (or `npx mettu check`) to catch:

- Missing or invalid `config.yaml`
- Missing templates referenced by `layout:`
- Duplicate output URLs across content files
- Missing local images or broken internal links

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

To use a custom domain such as `mettu-docs.hemanthandey.dev`, place it in `public/CNAME` and point your DNS `CNAME` record to `<username>.github.io`.

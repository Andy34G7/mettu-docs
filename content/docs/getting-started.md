---
layout: docs
title: "Getting Started"
section: "Overview"
order: 2
description: "Install @andy34g7/mettu, create the minimal folder structure, and run your first build."
---

## Prerequisites

- **Node.js** (v20+)
- **Python** (3.10+)

## Create a new site

Initialize a project and install `@andy34g7/mettu`:

```bash
mkdir my-site && cd my-site
npm init -y
npm install @andy34g7/mettu
```

Add the `mettu` CLI scripts to your `package.json`:

```json
{
  "name": "my-site",
  "private": true,
  "scripts": {
    "dev": "mettu dev",
    "build": "mettu build",
    "preview": "mettu preview",
    "new": "mettu new",
    "check": "mettu check",
    "clean": "mettu clean"
  },
  "dependencies": {
    "@andy34g7/mettu": "^2.0.0"
  }
}
```

## Minimal directory layout

```text
my-site/
├── config.yaml
├── package.json
├── assets/
│   └── css/
│       └── main.css
├── content/
│   └── index.md
└── public/          # Optional static files (CNAME, robots.txt, favicon.svg)
```

Create `assets/css/main.css` so Tailwind CSS v4 and DaisyUI v5 are wired up:

```css
@import "./generated.fonts.css";
@import "tailwindcss";
@plugin "@tailwindcss/typography";
@import "./generated.daisyui.css";
```

Create a minimal `config.yaml`:

```yaml
site_name: "My Site"
author: "Your Name"
base_url: "https://example.com"

theme:
  default: "dim"
  include:
    - "dim"
    - "light"
```

## Run the dev server

```bash
npm run dev
```

On first run, `mettu` creates a local `.venv/`, installs its Python requirements, generates your theme/font/syntax CSS, and starts Vite with live reload.

When you're ready for production:

```bash
npm run check
npm run build
```

Your static site is written to `dist/`.

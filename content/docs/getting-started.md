---
layout: docs
title: "Getting Started"
section: "Overview"
order: 2
description: "Install @andy34g7/mettu, create the minimal folder structure, and run your first dev server and build."
---

## Prerequisites

Make sure you have both installed:

- **Node.js** v20 or newer
- **Python** 3.10 or newer (with `venv` support)

## 1. Initialize your project

Create a folder for your site and install `@andy34g7/mettu`:

```bash
mkdir my-site && cd my-site
npm init -y
npm install @andy34g7/mettu
```

Update `package.json` so your npm scripts call the `mettu` CLI:

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

## 2. Add the starter files

You only need three files to get a working site: `config.yaml`, `assets/css/main.css`, and `content/index.md`.

**`config.yaml`**

```yaml
site_name: "My Site"
author: "Your Name"
base_url: "https://example.com"

theme:
  default: "dim"
  include:
    - "dim"
    - "light"

navigation:
  - name: "Home"
    url: "/"
  - name: "Docs"
    url: "/docs/"
```

**`assets/css/main.css`**

```css
@import "./generated.fonts.css";
@import "tailwindcss";
@plugin "@tailwindcss/typography";
@import "./generated.daisyui.css";
```

**`content/index.md`**

```markdown
---
layout: page
title: "Home"
---

# Hello!

This site is built with mettu.
```

## 3. Start developing

Run the dev server:

```bash
npm run dev
```

On the first run, `mettu` creates `.venv/` in your project root and installs its Python dependencies (`Markdown`, `Jinja2`, `PyYAML`, `Pygments`, `python-dotenv`). After that, edits to `content/`, `templates/`, `assets/`, or `config.yaml` rebuild and reload automatically.

## 4. Validate and build for production

Before deploying, run the site validator and production build:

```bash
npm run check
npm run build
```

`npm run check` catches missing templates, duplicate URLs, broken internal links, and invalid YAML. `npm run build` outputs the final static site into `dist/`. You can inspect it locally with `npm run preview`.

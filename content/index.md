---
layout: page
title: "Static site generation, without the friction"
description: "A hybrid Python and Vite static site generator for personal sites, blogs, and docs."
---

`mettu` is a static site generator built with Python and Vite. The goal is not to chase synthetic benchmarks, it is to make building and maintaining a static site genuinely pleasant: write Markdown, get Tailwind CSS v4 and DaisyUI v5 out of the box, see changes live while editing, and deploy plain static files.

Everything lives in one installable package (`@andy34g7/mettu`), so your site repository only holds your content, `config.yaml`, assets, and any templates you want to customize.

### Start here

- [Preface](/docs/): What `mettu` is, why it uses Python and Vite together, and how a build actually runs.
- [Getting Started](/docs/getting-started/): Install `@andy34g7/mettu`, set up the minimal files, and run your first dev server.
- [Project Structure](/docs/project-structure/): What every folder does, what gets generated, and what belongs in git.
- [Configuration](/docs/configuration/): Full `config.yaml` walkthrough for themes, fonts, syntax highlighting, Markdown, and feeds.
- [Templates & Layouts](/docs/templates-and-layouts/): Override built-in Jinja2 templates or build custom sections with dynamic collections.

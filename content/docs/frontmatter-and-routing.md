---
layout: docs
title: "Frontmatter & Routing"
section: "Authoring"
order: 1
description: "How content paths map to URLs, YAML frontmatter fields, automatic inference, drafts, and permalinks."
---

Every `.md` file inside `content/` maps to an HTML page. You can nest folders as deeply as you want, and `mettu` mirrors that folder structure into clean URLs.

## How file paths become URLs

| Source file | Output file | URL |
| :--- | :--- | :--- |
| `content/index.md` | `index.html` | `/` |
| `content/about.md` | `about/index.html` | `/about` |
| `content/blog/index.md` | `blog/index.html` | `/blog/` |
| `content/blog/crdt.md` | `blog/crdt/index.html` | `/blog/crdt` |
| `content/docs/getting-started.md` | `docs/getting-started/index.html` | `/docs/getting-started` |

If you want a file to live at a different URL than its path in `content/`, set `permalink` in frontmatter:

```yaml
---
title: "Not Found"
layout: page
permalink: "/404.html"
---
```

> [!NOTE]
> Two files resolving to the same URL will overwrite each other during a build. Run `npm run check` to catch duplicate URLs before deploying.

## Frontmatter fields

Frontmatter is optional YAML at the top of a Markdown file, wrapped in `---`:

```yaml
---
layout: post
title: "An Introduction to CRDTs"
date: 2026-06-17
tags:
  - data structures
  - distributed systems
description: "Why distributed systems run into conflicts and how CRDTs resolve them."
---
```

Supported fields:

| Field | Type | What it does |
| :--- | :--- | :--- |
| `layout` | string | Template name (without `.html`), such as `post`, `docs`, `page`, or any custom template you add. |
| `title` | string | Page title. Inferred from the first `# Heading` or filename if omitted. |
| `date` | date / string | Publication date. Collections with dates sort newest-first; also required for feed entries. |
| `order` | number | Numeric sort order when `date` is not present (used for `docs` navigation and ordered lists). |
| `section` | string | Groups documentation pages in the `docs.html` sidebar. |
| `tags` | list of strings | Generates `/tags/<slug>.html` pages and populates `{layout}_tags` and `all_tags`. |
| `description` | string | Meta description, RSS/Atom summary, and search excerpt. |
| `permalink` | string | Overrides the output path and URL. |
| `draft` | boolean | Set `true` (or `yes` / `1`) to skip rendering the page completely. |
| `feed` | boolean | Set `false` to keep a dated page out of RSS/Atom feeds. |
| `theme` | string | Overrides the DaisyUI `data-theme` attribute for this page only. |

You can also add any custom keys you want (like `hero_title`, `private`, or `spotify_id`) and read them inside your templates via `page.<key>`.

## Automatic inference

To keep authoring lightweight, `mettu` fills in common fields when you leave them out:

- **Title**: Uses the first `# Heading` in your Markdown body, or falls back to a title-cased version of the filename.
- **Layout**: Defaults by top-level folder (`posts/` uses `post`, `docs/` uses `docs`, everything else uses `page`). You can override these defaults in `config.yaml` with `default_layout` and `default_layouts`.
- **Metadata**: `page.reading_time` (estimated minutes at ~200 words/min) and `page.excerpt` (plain-text snippet) are computed automatically.

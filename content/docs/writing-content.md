---
layout: docs
title: "Writing Content"
section: "Authoring"
order: 1
description: "Frontmatter, automatic layout inference, callouts, tags, and table of contents."
---

Every `.md` file inside `content/` becomes a page in your site.

## Frontmatter

Frontmatter is YAML at the top of a Markdown file, fenced by `---`:

```yaml
---
layout: post
title: "Hello World"
date: 2026-11-13
tags:
  - notes
  - ssg
description: "A short summary for feeds and meta tags."
---
```

Common fields:

| Field | Purpose |
| :--- | :--- |
| `layout` | Template name (without `.html`), e.g. `post`, `docs`, `page`. |
| `title` | Page title. Inferred from the first `# Heading` or filename if omitted. |
| `date` | Used for sorting collections and populating feeds. |
| `order` | Numeric sort order when `date` is absent (used heavily in `docs`). |
| `section` | Groups documentation pages in the left sidebar (`docs` layout). |
| `tags` | List of tags. Generates `/tags/<slug>.html` and `{layout}_tags`. |
| `permalink` | Custom output URL (e.g. `/about/` or `/404.html`). |
| `draft` | Set `true` to exclude the page from builds. |
| `feed` | Set `false` to exclude a dated page from RSS/Atom feeds. |
| `theme` | Override the DaisyUI `data-theme` for a single page. |

## Automatic inference

You don't have to repeat boilerplate on every file:

- **Title**: If `title` is omitted, `mettu` uses the first `# Heading` in the file, or falls back to a title-cased filename.
- **Layout**: Defaults by directory (`posts/` → `post`, `docs/` → `docs`, otherwise `page`), configurable via `default_layout` / `default_layouts` in `config.yaml`.
- **Reading time & excerpt**: `page.reading_time` and `page.excerpt` are computed automatically.

## GitHub-style callouts

When using `layout: docs` (or any Markdown content), blockquotes starting with a tag render as styled alert boxes:

```markdown
> [!NOTE]
> Useful background context.

> [!TIP]
> A helpful shortcut or best practice.

> [!IMPORTANT]
> Key information the reader shouldn't miss.

> [!WARNING]
> Something that could break if overlooked.

> [!CAUTION]
> High-risk action or destructive operation.
```

## Table of Contents

Enable `toc` in `config.yaml` under `markdown.extensions`:

```yaml
markdown:
  extensions:
    - toc:
        permalink: true
```

`mettu` populates both `page.toc` (rendered HTML list) and `page.toc_tokens` (structured heading tree used by `docs.html` and `/search-index.json`).

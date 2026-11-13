---
layout: docs
title: "Templates & Layouts"
section: "Reference"
order: 2
description: "Override built-in Jinja2 templates, build custom layouts, and use dynamic collection variables."
---

`mettu` uses Jinja2 with a `ChoiceLoader` that checks your site's `templates/` directory first and falls back to `mettu`'s built-in templates.

## Overriding vs. inheriting

You only need to create files in `templates/` for layouts you want to customize:

```text
my-site/
└── templates/
    ├── blog.html       # Overrides built-in blog.html
    └── music.html      # Adds a brand-new custom layout
```

Built-in templates you don't override (`base.html`, `docs.html`, `page.html`, `post.html`, `tags.html`, `feed.xml.j2`, `sitemap.xml.j2`) are inherited directly from `@andy34g7/mettu`.

## Context variables

Every template receives:

- `site` — Parsed `config.yaml` dictionary.
- `page` — Current page's frontmatter + inferred fields (`url`, `title`, `reading_time`, `excerpt`, `toc`, `toc_tokens`, `breadcrumbs`, `prev_page`, `next_page`).
- `content` — Rendered HTML body (render with `{{ content | safe }}`).
- **Dynamic collections**: For every `layout: <name>` used in `content/`, `mettu` exposes:
  - `<name>s` (e.g., `posts`, `docs`, `tils`) — List of pages sorted by `date` (newest first) or `order` (ascending). Hyphens become underscores (`my-note` → `my_notes`).
  - `<name>_tags` (e.g., `post_tags`, `til_tags`) — Sorted list of unique tags used in that layout.
- `all_tags` — Dictionary mapping each tag name to its list of pages.
- `docs_tree` — Hierarchical section tree for `layout: docs`.

> [!NOTE]
> Jinja2 `autoescape` is enabled for `.html`, `.htm`, `.xml`, and `.j2` templates. If a frontmatter field intentionally contains HTML (such as a `<br>`), render it with `| safe`.

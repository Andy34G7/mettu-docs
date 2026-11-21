---
layout: docs
title: "Templates & Layouts"
section: "Reference"
order: 2
description: "Override built-in Jinja2 templates, build custom layouts, and use dynamic collection variables."
---

`mettu` renders every page with Jinja2 using a `ChoiceLoader`: it checks your site's `templates/` folder first, and falls back to the built-in templates shipped inside `@andy34g7/mettu`.

## Override only what you customize

You do not need to copy all of `mettu`'s templates into your repo. Create a `templates/` directory only for files you want to override or add:

```text
my-site/
└── templates/
    ├── base.html        # Overrides mettu's base layout
    ├── blog.html        # Overrides the blog index
    ├── til.html         # New custom layout for Today I Learned entries
    └── til_list.html    # New index layout for /til/
```

Any template you do not override (`docs.html`, `page.html`, `post.html`, `tags.html`, `feed.xml.j2`, `sitemap.xml.j2`) is inherited automatically from `@andy34g7/mettu`, so you get improvements whenever you run `npm update @andy34g7/mettu`.

## Context variables

Every template receives:

| Variable | Description |
| :--- | :--- |
| `site` | The parsed `config.yaml` dictionary. |
| `page` | Current page's frontmatter plus computed fields (`url`, `title`, `reading_time`, `excerpt`, `toc`, `toc_tokens`, `breadcrumbs`, `prev_page`, `next_page`). |
| `content` | Rendered HTML body of the Markdown file. Always render with `{{ content | safe }}`. |
| `<layout>s` | Dynamic list of all pages using `layout: <layout>` (for example `posts`, `tils`, `docs`). Hyphens become underscores (`my-note` becomes `my_notes`). Sorted by `date` (newest first) or `order` (ascending). |
| `<layout>_tags` | Sorted list of unique tags used across pages of that layout (for example `post_tags`, `til_tags`). |
| `all_tags` | Dictionary mapping every tag string to its list of pages (`{tag: [page, ...]}`). |
| `docs_tree` | Grouped section tree for `layout: docs`. |

## Example: adding a custom "TIL" section

Suppose you want a `/til/` section with its own tag filter bar:

1. Create `templates/til.html` for individual entries (extending `base.html`).
2. Create `templates/til_list.html` for the index page at `content/til/index.md` (`layout: til_list`):

```html
{% extends "base.html" %}

{% block content %}
<h1 class="text-4xl font-bold mb-6">{{ page.title }}</h1>

{% if til_tags %}
<div class="flex flex-wrap gap-2 mb-6">
  {% for tag in til_tags %}
  <span class="badge badge-outline">{{ tag }}</span>
  {% endfor %}
</div>
{% endif %}

<ul class="space-y-4">
  {% for item in tils %}
  <li>
    <a href="{{ item.url }}/" class="link link-primary font-semibold">{{ item.title }}</a>
    <span class="text-sm opacity-60 ml-2">{{ item.date }}</span>
  </li>
  {% endfor %}
</ul>
{% endblock %}
```

3. Add Markdown files in `content/til/` with `layout: til` and `date:` / `tags:`. `mettu` automatically populates `tils` and `til_tags` without any Python changes.

## Watch out for Jinja2 autoescape

> [!WARNING]
> `mettu` enables Jinja2 `autoescape` for `.html`, `.htm`, `.xml`, and `.j2` files. That prevents accidental XSS, but it also means any frontmatter string containing raw HTML (like `<br>` in a subtitle) will be escaped unless you explicitly write `{{ page.hero_subtitle | safe }}`.

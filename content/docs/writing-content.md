---
layout: docs
title: "Writing Content & Docs"
section: "Authoring"
order: 2
description: "Markdown extensions, syntax highlighting, GitHub-style callouts, Table of Contents, and documentation portals."
---

`mettu` uses Python-Markdown under the hood, with Pygments for syntax highlighting and a few extra conveniences built in for technical writing and documentation sites.

## Markdown extensions

By default, `mettu` enables a sensible set of extensions: `fenced_code`, `codehilite`, `footnotes`, `tables`, `attr_list`, `sane_lists`, `md_in_html`, and `toc` (with `permalink: true`).

You can customize the list in `config.yaml` under `markdown.extensions`. For example, adding `nl2br` turns single newlines into `<br>` tags, which is handy for poetry or short notes.

Code blocks use standard fenced syntax with a language tag:

````markdown
```python
def greet(name: str) -> str:
    return f"Hello, {name}"
```
````

Pygments highlights the block using the `syntax.pygments_theme` from `config.yaml`, and in the `docs` layout every code block automatically gets a one-click **Copy** button.

## GitHub-style callouts

Blockquotes starting with `[!NOTE]`, `[!TIP]`, `[!IMPORTANT]`, `[!WARNING]`, or `[!CAUTION]` are automatically transformed into styled DaisyUI alerts:

```markdown
> [!NOTE]
> Useful context that helps explain a detail.

> [!TIP]
> A practical shortcut or recommended pattern.

> [!WARNING]
> Something that can trip you up if you miss it.
```

Here is how a tip renders:

> [!TIP]
> Callouts work in any Markdown file, not just `layout: docs`.

## Table of Contents

When `toc` is enabled in `markdown.extensions`, `mettu` extracts headings for every page and exposes two fields on `page`:

- `page.toc`: Ready-to-render HTML (`<ul>...</ul>`) that you can drop into any template with `{{ page.toc | safe }}`.
- `page.toc_tokens`: Structured nested list of heading objects (`id`, `name`, `level`, `children`), used by `docs.html` for the right-hand "On this page" sidebar and by `search-index.json` so headings are searchable.

## Building a documentation portal

To create a docs section like this site:

1. Put your Markdown files in `content/docs/` (or set `layout: docs` in frontmatter).
2. Group pages with `section:` and sort them with `order:`:

```yaml
---
layout: docs
title: "Configuration"
section: "Reference"
order: 1
---
```

`mettu` automatically builds:

- The left sidebar tree (`docs_tree`) grouped by `section` and ordered by `order`.
- Breadcrumbs (`page.breadcrumbs`).
- Sequential **Previous / Next** pagination links (`page.prev_page`, `page.next_page`).
- Instant client-side search (`Ctrl+K` / `Cmd+K`) backed by `/search-index.json`, indexing titles, sections, tags, headings, and excerpts.

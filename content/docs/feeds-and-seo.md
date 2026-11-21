---
layout: docs
title: "Feeds & SEO"
section: "Features"
order: 3
description: "Atom and RSS feeds, pretty-feed styling, XML sitemaps, and client-side search index generation."
---

Every full build automatically generates feeds, a sitemap, and a search index without extra plugins.

## Atom / RSS feeds

Configure one or more feeds under `feeds` in `config.yaml`:

```yaml
feeds:
  - filename: "feed.xml"
    layouts: ["post", "til"]
    title: "My Site"
    subtitle: "All posts and notes"
    content: false
  - filename: "blog/feed.xml"
    layouts: ["post"]
    title: "My Site - Blog"
    subtitle: "Long-form posts"
    content: true
```

A few details to keep in mind:

- Only pages with a valid `date:` in frontmatter are included in feeds.
- To hide a specific dated post from feeds, add `feed: false` to its frontmatter.
- If `feeds` is omitted from `config.yaml`, `mettu` falls back to generating `feed.xml` (`post` + `til`), `blog/feed.xml` (`post`), and `til/feed.xml` (`til`).
- Drop `pretty-feed-v3.xsl` into `public/` if you want browsers to render your XML feed as a readable web page instead of raw XML.

## Sitemap

`mettu` writes `/sitemap.xml` using `base_url` from `config.yaml` and includes a `<lastmod>` timestamp for every non-draft page (using the page's frontmatter `date` or falling back to file modification time).

## Search index

Every build generates `/search-index.json` containing each page's `title`, `url`, `section`, `layout`, `tags`, flattened `headings`, and `excerpt`. The built-in `docs.html` template loads this file for its `Ctrl+K` / `Cmd+K` search modal, and you can also fetch it in your own custom templates to add site-wide search anywhere.

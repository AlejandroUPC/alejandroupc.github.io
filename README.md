# alejandroupc.github.io

## Adding a series

A series landing page is a regular post with the `series` layout. Keeping it in `_posts` makes the series appear in the homepage, Blog, tags, pagination, and RSS feed.

```yaml
---
title: Learning RAG
layout: series
series: learning-rag
permalink: /series/learning-rag/
date: 2026-09-10
tags: [rag, llm, ai]
---
```

The `series` value is the stable key connecting the landing page to its nested posts.

An optional `parts` list in the landing page can define a complete roadmap before every post exists. Each roadmap entry is matched to a published series post by its numeric `order`; planned entries automatically become links as their documents are added.

Add each nested post under `_series_posts/<series-key>/`. These posts are listed on the series page but do not appear independently on the homepage or main Blog page.

```yaml
---
title: What is RAG?
series: learning-rag
order: 1
date: 2026-09-11
tags: [rag, llm, ai]
---
```

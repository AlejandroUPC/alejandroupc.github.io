---
layout: default
title: Starting a blog — clean draft
permalink: /00_set_up/starting-a-blog-clean/
---

# Starting a blog

> A clearer, typo‑free and easier‑to‑scan version of your original post, keeping the same spirit and steps.

## TL;DR

- Host a personal site with GitHub Pages + Jekyll
- Add a simple `index.html` and basic CSS
- Use Jekyll posts, a blog index, and an RSS feed
- Run locally with Docker for fast feedback

## Contents

{:toc}

## Introduction

I wanted a simple, fast way to write and share notes without paywalls or heavy tooling. GitHub Pages + Jekyll is a great fit: free, minimal, and easy to version‑control.

References I used:

- Creating an Engineering Blog with GitHub Pages
- Creating and Hosting a Personal Site on GitHub
- Build A Blog With Jekyll And GitHub Pages

## Git and GitHub in one minute

- Git is a version control system to track file changes and history.
- GitHub hosts your repositories online and makes collaboration easy.
- Learn basic Git (clone, branch, commit, push); it pays off quickly.

## GitHub Pages

GitHub Pages turns a repository into a website. Why it’s nice:

- Free hosting for static sites
- No database or server to manage
- Works perfectly with Jekyll

## Create the repository

1. Create a new public repo named `yourusername.github.io`.
2. Add a README (optional) and clone locally.
3. Add a very simple `index.html` to the repo root:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <title>Hello</title>
  </head>
  <body>
    <h1>Welcome</h1>
  </body>
</html>
```

Push, then visit `https://yourusername.github.io`.

## Add a touch of CSS

Create `css/main.css` for site‑wide styles:

```css
body {
  margin: 0;
  font-family: system-ui, -apple-system, Segoe UI, Roboto, Ubuntu, Cantarell, Noto Sans, sans-serif;
  line-height: 1.6;
}
h1 { font-size: 2rem; }
```

Reference it in `index.html`:

```html
<link rel="stylesheet" href="/css/main.css" />
```

## Jekyll basics

Jekyll turns folders and Markdown into a website. Key pieces:

- `_posts/` holds blog posts named `YYYY-MM-DD-title.md`
- `_layouts/` defines templates (e.g., `default.html`, `post.html`)
- `_config.yml` sets site options (title, plugins, permalinks)

Minimal `_config.yml` example:

```yml
title: "My Blog"
plugins:
  - jekyll-feed
  - jekyll-seo-tag
```

## Your first post

Create `_posts/2025-09-21-first-post.md`:

```markdown
---
layout: post
title: "We try our first post"
date: 2025-09-21
tags: [jekyll, setup]
---

Hello world — this is my first post.
```

## Blog index page

Add `blog/index.html` to list your posts:

```html
---
layout: default
title: Blog
---
<h1>{{ page.title }}</h1>
<ul>
  {% for post in site.posts %}
    <li>
      <span>{{ post.date | date: "%b %-d, %Y" }}</span>
      <a href="{{ post.url }}">{{ post.title }}</a>
    </li>
  {% endfor %}
  </ul>
```

## Permalinks (optional)

Add this to `_config.yml` to include `/blog/` in post URLs:

```yml
permalink: /blog/:year/:month/:day/:title
```

## RSS feed

We already included `jekyll-feed`. It generates `/feed.xml` automatically — no need to hand‑roll Atom.

## Local development with Docker

Dockerfile:

```dockerfile
FROM ruby:3.1-slim
ENV LANG=C.UTF-8 \
    LC_ALL=C.UTF-8 \
    DEBIAN_FRONTEND=noninteractive
RUN apt-get update && apt-get install -y \
    build-essential git curl \
  && rm -rf /var/lib/apt/lists/*
WORKDIR /usr/src/app
COPY Gemfile* ./
RUN gem install bundler && bundle install
COPY . .
EXPOSE 4000
CMD ["bundle", "exec", "jekyll", "serve", "--host", "0.0.0.0", "--watch"]
```

`Gemfile`:

```ruby
source "https://rubygems.org"

gem "jekyll"
group :jekyll_plugins do
  gem "jekyll-feed"
  gem "jekyll-seo-tag"
end
```

Build and run:

```sh
docker build -t jekyll-site .
docker run -p 4000:4000 -v "$PWD":/usr/src/app jekyll-site
```

Open `http://localhost:4000/blog` to preview.

## Closing

That’s the minimal setup. From here you can tweak the layout, add tags, search, and a nicer theme. Small steps go a long way.

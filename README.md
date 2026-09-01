# blog

Source for my blog, built with plain [Jekyll](https://jekyllrb.com) (no third-party theme) and hosted on GitHub Pages.

## Writing a new post

Add a file to `_posts/` named `YYYY-MM-DD-slug.md`:

```markdown
---
layout: post
title: "Post title"
tags: [tag1, tag2]
excerpt_separator: <!--more-->
---

The excerpt shown on the homepage goes here.
<!--more-->

The rest of the post goes here.
```

Add `thumbnail: /assets/img/whatever.jpg` to the front matter to show an image next to the excerpt on the homepage.

## Customizing

- Site title, tagline: `_config.yml`
- Nav links: `_includes/header.html`
- Colors, fonts, layout: `assets/css/style.css`

Push to `main` — GitHub Pages rebuilds automatically within a minute or two.

## Running locally

```
bundle install
bundle exec jekyll serve
```

Then open `http://localhost:4000`.

## Attaching a custom domain

Once a domain is registered:

1. Add a `CNAME` file to the repo root containing just the domain, e.g. `blog.example.com`.
2. At the domain registrar, add a `CNAME` DNS record pointing `blog` → `ri-sh.github.io`.
3. In the repo's Settings → Pages, enter the custom domain and enable "Enforce HTTPS" once it verifies.

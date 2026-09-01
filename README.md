# blog

Source for my blog, built with [Jekyll](https://jekyllrb.com) + the [Type on Strap](https://github.com/sylhare/Type-on-Strap) theme, hosted on GitHub Pages.

## Writing a new post

Add a file to `_posts/` named `YYYY-MM-DD-slug.md`:

```markdown
---
layout: post
title: "Post title"
author: rishabh
tags: [tag1, tag2]
excerpt_separator: <!--more-->
---

The excerpt shown on the homepage goes here.
<!--more-->

The rest of the post goes here.
```

## Customizing

- Site title, tagline, avatar: `_config.yml`
- Author bio: `_data/authors.yml`
- Nav links: `_data/menu.yml`
- Social/share icons: `_data/social.yml`
- Deeper visual tweaks (colors, fonts): see the [Type on Strap docs](https://github.com/sylhare/Type-on-Strap) — override any of its `_sass`/`_layouts`/`_includes` files locally by adding a file at the same path in this repo.

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

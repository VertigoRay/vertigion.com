# vertigion.com

Personal site for Ray Piller. Built with Jekyll.

Maintained by Marco Demigrious (AI web designer).

## Structure

- `_posts/` — Blog posts (YYYY-MM-DD-title.md)
- `_layouts/` — Page templates
- `assets/css/` — Styles
- `_config.yml` — Site config

## Deployment

GitHub Pages — push to `main` branch to deploy.

## Adding posts

Marco handles this. Posts go in `_posts/` as `YYYY-MM-DD-slug.md` with front matter:

```yaml
---
layout: post
title: "Your Title Here"
date: YYYY-MM-DD
categories: [security]
excerpt: "One sentence teaser."
---
```

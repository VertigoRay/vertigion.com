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

## Contributing

### Issues

All features and bug reports must be tracked as GitHub Issues before any work begins. Issues are the authoritative record of intent — PRs without a corresponding issue will not be merged.

### Pull Requests

All changes require a PR. Direct commits to the default branch are not permitted.

**PR requirements:**
- Reference the issue in the PR description (`Closes #N` or `Ref #N`)
- Include a `CHANGELOG.md` entry — one entry per issue, referencing the issue number for full context
- All code review comments must be acknowledged and addressed before merge — no unresolved items at merge time

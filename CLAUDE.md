# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Site Overview

Personal website for Satyam Kapoor (satyamkapoor.com) — a Hugo static site using the `hello-friend-ng` theme, deployed to GitHub Pages via GitHub Actions.

## Commands

```bash
# Local development server with live reload
hugo server -D

# Build the site (output goes to public/)
hugo --minify

# Create a new post
hugo new posts/my-post-title.md
```

Hugo version in CI: **0.147.8**. The `public/` directory is gitignored — it's the build output.

## Deployment

Pushing to `main` triggers `.github/workflows/` which runs `hugo --minify` and deploys `./public` to GitHub Pages. The theme (`themes/hello-friend-ng`) is a git submodule — clone with `--recurse-submodules`.

## Content Structure

All posts live in `content/posts/` as Markdown files with YAML/TOML frontmatter. Required frontmatter fields:

```yaml
---
title: "Post Title"
date: 2026-01-15
draft: false
tags:
  - kubernetes
categories:
  - tech
description: "Short description for SEO"
summary: "Shown in post listings"
---
```

- `draft: true` posts are excluded from the production build (included with `hugo server -D`)
- Taxonomies: `tags` and `categories`
- Static assets (images, standalone HTML files) go in `static/` — they're served at the root path

## Architecture Notes

- `config.toml` controls all site-wide settings: theme, menus, social links, pagination
- The `hello-friend-ng` theme provides all layouts/templates — don't modify files inside `themes/` directly; use Hugo's override mechanism by mirroring the path under the root `layouts/` directory instead
- Standalone HTML files in `static/` (e.g., `european_driving_handbook.html`) are served directly without Hugo templating
- Disqus comments are enabled via `shortname = "satyamkapoor-2"` in config

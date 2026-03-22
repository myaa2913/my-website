# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

Personal website for Matt Corritore — a Hugo static site using the [hugo-bearblog](https://github.com/janraasch/hugo-bearblog) theme, deployed to Cloudflare Pages at `mattcorritore.com`.

## Commands

```bash
# Local development server
hugo server

# Build for production (same command Cloudflare runs)
hugo --minify

# Create a new blog post
hugo new content/blog/post-name.md
```

Hugo version: **0.158.0** (set in `.cloudflare/pages.toml`)

## Architecture

- **`hugo.toml`** — site config (baseURL, title, params like description/author/footer)
- **`content/`** — all Markdown content
  - `_index.md` — homepage body text
  - `about.md` — about page
  - `blog/` — blog posts (TOML front matter with `title`, `date`, `description`, `tags`)
- **`themes/hugo-bearblog/`** — git submodule; do not edit directly
- **`archetypes/default.md`** — template used by `hugo new`; new content defaults to `draft = true`
- **`public/`** — build output; not committed
- **`.cloudflare/pages.toml`** — Cloudflare Pages build config

## Theme customization

To override theme templates without modifying the submodule, mirror the path under `layouts/`. For example, to override `themes/hugo-bearblog/layouts/partials/footer.html`, create `layouts/partials/footer.html`. Hugo's lookup order prefers the site's own `layouts/` over the theme's.

Custom CSS/JS can be injected via `layouts/partials/custom_head.html` and `layouts/partials/custom_body.html` (these are empty partials in the theme intended for this purpose).

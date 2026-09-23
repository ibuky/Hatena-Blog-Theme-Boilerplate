# AGENTS.md

## Project

Hatena Blog theme boilerplate. A single SCSS file compiled to CSS for customization of [はてなブログ](https://blog.hatenablog.com/) custom themes. MIT license (Hatena Co., Ltd).

## Commands

| Command                 | What it does                                                     |
| ----------------------- | ---------------------------------------------------------------- |
| `npm install`           | Install deps (sass, vite, autoprefixer, normalize.css)           |
| `npm start -- <domain>` | Dev server (requires blog domain, e.g. `example.hatenablog.com`) |
| `npm run build`         | Compile SCSS → `build/boilerplate.css`                           |

**No test, lint, or typecheck commands exist.**

## Architecture

- **Entry point:** `scss/boilerplate.scss` — imports normalize.css, `_variable`, `_core`
- **Output:** `build/boilerplate.css` (single file, no minification)
- **PostCSS plugin:** autoprefixer only
- **No build artifacts to clean** — `build/` is gitignored

## Dev server quirks

The dev server (`server.js`) requires a blog domain as argv[2]. It:

1. Sets CORS origin to `https://<domain>`
2. Adds `access-control-allow-private-network: true` header (needed for localhost access from blog)

Usage: `npm start -- example.hatenablog.com`

## Blog setup (for dev)

When developing a theme on a real blog, the blog's Design CSS must contain exactly:

```css
/* Responsive: yes */
```

And `<head>` metadata must include the Vite client script and SCSS stylesheet URLs. See README for full instructions.

## SCSS conventions

- `_variable.scss`: design tokens (colors, fonts, breakpoints)
- `_core.scss`: base styles for blog elements (post body, comments, sidebar, etc.)
- Breakpoint variables: `$mq-xs` (480px), `$mq-sm` (768px), `$mq-md` (992px), `$mq-lg` (1200px)
- Font stack includes Japanese fonts: `Hiragino Kaku Gothic Pro`, `Meiryo`, `MS PGothic`

## Current Session (2026-09-23)

### Goal
GitHub README-like dark design theme for "おうちLLM" (`llm-notes.hateblo.jp`, custom domain `ai.taneyats.com`).

### Design Decisions
- **Color palette:** GitHub Dark (`#0d1117` bg, `#161b22` surface, `#c9d1d9` text, `#f0f6fc` headers, `#58a6ff` links)
- **Syntax highlighting:** Catppuccin Mocha (see `_variable.scss` lines 33–41)
- **Border radius:** 8px on `.entry`, `.hatena-module`; 6px on code/tables/blockquotes
- **Code blocks:** `white-space: pre` (no-wrap), `max-height: 280px` with scroll, Catppuccin Mocha tokens
- **Blockquote:** `border-left: 5px solid $link` accent, `border-radius: 6px`, matching code bg/border
- **Sidebar (`aside#box2`):** same bg as entry, no border, rounded corners, `margin-left: 1em` gap from entry
- **Scrollbar:** 6px width, dark track/thumb matching theme colors

### Key Selectors (current state)
| Selector | Purpose |
|---|---|
| `.entry` | Card wrapper — bg, border, radius |
| `.entry-content` | Article body — inherits bg from `.entry` |
| `.hatena-module` | Sidebar modules — bg, no border, radius |
| `#box2` | Sidebar container — bg override |
| `pre`, `code` | Code blocks — scroll, no-wrap, Catppuccin colors |
| `blockquote` | Quotes — left accent line, code-like styling |
| `a.leave-comment-title` | Comment button — dark bg, `!important` overrides |
| `.hatena-follow-button` | "読者になる" button — 32px height |
| `.search-module-input` | Search input — 36px height |
| `#footer`, `#footer-inner` | Footer — dark bg |

### Files Modified
- `scss/lib/_variable.scss` — all design tokens (colors, syntax highlighting)
- `scss/lib/_core.scss` — all component styles (~730 lines)
- `build/boilerplate.css` — compiled output (last built: 19.5+ kB)

### Blog Dev Setup
- Design CSS must contain `/* Responsive: yes */`
- `<head>` needs Vite client + SCSS stylesheet URLs
- Dev server: `npm start -- llm-notes.hateblo.jp`

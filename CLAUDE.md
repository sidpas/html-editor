# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this project is

A single-file, zero-dependency HTML editor that runs entirely in the browser. Users open a local HTML file, edit text inline via `contentEditable`, and save back to disk using the File System Access API. No build step, no framework, no backend.

Live at: **https://html-edit.com**  
Repo: **https://github.com/sidpas/html-editor**  
Deployed via: GitHub Pages (main branch root), with Cloudflare in front for SSL and analytics.

## File overview

- `index.html` — the entire editor application (HTML + CSS + JS in one file)
- `landing.html` — a separate doodle-aesthetic landing page (not currently linked from the main page)
- `favicon.svg` — the `</>` logo mark used as browser favicon
- `CNAME` — contains `html-edit.com` for GitHub Pages custom domain routing
- `sitemap.xml` — single-URL sitemap pointing to `https://html-edit.com/`

## Architecture of index.html

Everything lives in one file. The structure is:

1. **`<head>`** — SEO meta tags, Open Graph, JSON-LD structured data, all inline CSS
2. **`<body>`** — toolbar (`#toolbar`), unsupported-browser banner (`#unsupported`), frame wrapper (`#frameWrap`)
3. **`<script>`** — all application logic

### Empty state vs loaded state

`#frameWrap` starts with a `.hint` div (hero section + keyboard shortcuts). When a file is loaded, the frameWrap is cleared and an `<iframe>` is appended using `appendChild`. The iframe renders the user's HTML file via `doc.open() / doc.write() / doc.close()`.

### Key JS functions

- `loadHtmlString(html, name)` — renders HTML into the iframe, enables toolbar buttons, auto-triggers `toggleEdit()` so editing is on by default
- `toggleEdit()` — flips `editing` boolean, sets `body.contentEditable`, applies/removes the dashed purple outline (`2px dashed #7c6cff`), focuses body
- `currentHtml()` — clones the iframe DOM, strips `contenteditable` attributes and the outline style before returning clean HTML
- `save()` — writes via File System Access API if available, falls back to `saveAs()`
- `saveAs()` — uses `showSaveFilePicker` or creates a download blob for non-Chromium browsers

### State variables

```js
let fileHandle   // FileSystemFileHandle | null
let editing      // boolean
let dirty        // boolean — true after any input event in the iframe
let iframe       // the active <iframe> element | null
```

## Deployment

Push to `main` — GitHub Pages deploys automatically. Cloudflare sits in front (proxied, SSL mode: Full). No build step required.

```bash
git add <files>
git commit -m "description"
git push
```

Hard refresh to see changes in browser: `Cmd+Shift+R`.

## Design decisions to preserve

- **Accent color**: `#7c6cff` (purple) — used for the edit outline, logo, and primary buttons. Do not change without updating both CSS variables and the `toggleEdit()` hardcoded color in JS.
- **Edit on by default**: `loadHtmlString` calls `toggleEdit()` after setting `editing = false`, so the first toggle flips it on. This is intentional.
- **Clean HTML on save**: `currentHtml()` must always strip `contenteditable` and `outline` before saving — never skip this.
- **File System Access API guard**: `supportsFS` check gates all direct save functionality. The fallback (blob download) must remain working for Safari/Firefox users.

## Positioning

Targeted at Claude Code / Cursor / v0 users who want to tweak AI-generated HTML without re-prompting. Tagline: *"The fastest way to tweak AI-generated HTML."*

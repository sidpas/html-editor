# HTML Inline Editor

A single-file, client-side tool to open an HTML file, edit its text inline, and save it back to disk. Everything runs in the browser — no files are uploaded anywhere.

> Save-to-disk requires a Chromium browser (Chrome/Edge/Opera) over HTTPS. Safari/Firefox fall back to download.

## Deploy to GitHub Pages

1. Create a new repo on GitHub (e.g. `html-editor`).
2. Push these files (`index.html` is the entry point):
   ```bash
   cd html-editor-site
   git init
   git add .
   git commit -m "HTML inline editor"
   git branch -M main
   git remote add origin https://github.com/USERNAME/REPO.git
   git push -u origin main
   ```
3. On GitHub: **Settings → Pages → Build and deployment → Source: Deploy from a branch**, pick `main` / `root`, save.
4. Wait ~1 min. Your site is live at `https://USERNAME.github.io/REPO/`.

## Custom domain (optional)

1. Buy a domain from any registrar.
2. **Settings → Pages → Custom domain**, enter it, save.
3. DNS at your registrar:
   - Subdomain (`editor.example.com`): `CNAME` → `USERNAME.github.io`
   - Apex (`example.com`): four `A` records → `185.199.108.153`, `185.199.109.153`, `185.199.110.153`, `185.199.111.153`
4. Tick **Enforce HTTPS** once the cert provisions.

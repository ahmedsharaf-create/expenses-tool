# Expense Ledger Consolidator

A single-page, client-side tool that merges multiple branch expense workbooks
(`.xlsx`) into one combined register, with a summary by branch, and lets you
download the result — all in the browser. No server, no backend, no data
ever leaves the user's machine.

**Live app:** it's just `index.html` — open it directly or deploy as a static site (instructions below).

## Expected input format

Each uploaded workbook can contain one or more sheets (typically one sheet
per branch/shop). Each sheet should have these columns, in this order:

| Date | Shop Name | Category | Subcategory | Amount | Description |
|------|-----------|----------|-------------|--------|--------------|

- If row 1 contains these headers, they're detected automatically (case-insensitive).
- If row 1 has no headers and data starts immediately, the tool falls back to
  assuming this exact column order.
- Empty rows are skipped automatically.

## Features

- Drag-and-drop or click to upload multiple `.xlsx` files at once
- Reads every sheet in every workbook and tags each row with its source
  workbook + branch (sheet name)
- Live stats: total entries, branches, workbooks, total amount
- Search/filter the combined table in-browser
- Download the merged result as `.xlsx` with:
  - a `Combined` sheet (every row)
  - a `Summary by Branch` sheet (totals per branch)

## Run locally

No build step needed — it's a single static HTML file.

```bash
# Option 1: just open it
open index.html          # macOS
start index.html         # Windows
xdg-open index.html      # Linux

# Option 2: serve it (recommended, avoids some browser file:// restrictions)
npx serve .
# or
python3 -m http.server 8080
```

Then visit the printed local URL.

## Deploy to GitHub Pages

1. Create a new GitHub repo and push this folder:

   ```bash
   git init
   git add .
   git commit -m "Initial commit: expense ledger consolidator"
   git branch -M main
   git remote add origin https://github.com/<your-username>/<your-repo>.git
   git push -u origin main
   ```

2. In the repo on GitHub: **Settings → Pages**.
3. Under **Build and deployment → Source**, choose one of:

   - **GitHub Actions** (recommended — already set up for you): this repo
     includes `.github/workflows/deploy.yml`, which auto-deploys on every
     push to `main`. Just select "GitHub Actions" as the source and push —
     no further setup needed.
   - **Deploy from a branch**: choose branch `main`, folder `/ (root)`, then **Save**.

4. GitHub will publish it at:
   `https://<your-username>.github.io/<your-repo>/`

   (First deploy can take a minute or two. Check the **Actions** tab for progress if using the Actions method.)

## Deploy elsewhere

This is a static site with a single external dependency loaded from a CDN
(SheetJS, via `cdnjs.cloudflare.com`). It will run unmodified on any static
host:

- **Netlify / Vercel**: drag-and-drop the folder in their dashboard, or connect the GitHub repo for auto-deploys.
- **Cloudflare Pages**: connect the repo, build command: none, output directory: `/`.
- **Any web server**: just copy `index.html` to the document root.

## Customizing the column mapping

If a branch uses different column names or order, edit the `HEADER_ALIASES`
and `STANDARD_ORDER` constants near the top of the `<script>` block in
`index.html`.

## Privacy

All parsing and file generation happens client-side in the browser using
the SheetJS library. Uploaded files are never sent to a server.

## License

MIT — see `LICENSE`.

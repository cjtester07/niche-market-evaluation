# AI Agency Niche Evaluator

A standalone tool for scoring AI agency niches using the 7-point framework from Luke Pierce (Boom Automations).

---

## Changelog

| CR    | Version | Date                    | Files Changed                                              | Summary                                                                          |
|-------|---------|-------------------------|------------------------------------------------------------|----------------------------------------------------------------------------------|
| CR001 | v1.0    | 2026-06-06 @ 20:00 EST  | index-v1.html, analyze.js, netlify.toml                    | Initial build — 7-point scorer, AI analysis via Netlify Function                 |
| CR002 | v2.0    | 2026-06-06 @ 21:00 EST  | index-v2.html, analyze.js, netlify.toml                    | PDF export, mobile/tablet responsive layout, version headers                     |
| CR003 | v3.0    | 2026-06-06 @ 22:55 EST  | index-v2.html (header), analyze.js (header), netlify.toml  | Naming convention docs, EST timestamps, correct zip structure                    |
| CR004 | v4.0    | 2026-06-06 @ 23:49 EST  | index-v3.html                                              | TOP STRENGTHS green highlight, BIGGEST RISKS red highlight (web + PDF)           |
| CR005 | v5.0    | 2026-06-07 @ 00:09 EST  | netlify.toml                                               | Added [[redirects]] rewrite so versioned index files are served at root — no more manual renaming |

---

## File Naming Convention

### HTML / CSS / JS assets
Version suffix goes in the **filename**:
```
index-v1.html       ← CR001 initial build
index-v2.html       ← CR002 PDF export + mobile responsive
index-v3.html       ← CR004 section color highlights
```

Netlify cannot serve versioned filenames at the root automatically. A `[[redirects]]` rewrite in `netlify.toml` maps `/` to the current active version. **On each new CR that produces a new index file, update the `to` field in `netlify.toml`:**

```toml
[[redirects]]
  from   = "/"
  to     = "/index-v4.html"   ← update this line only
  status = 200
```

### Netlify Functions
Netlify resolves functions by **filename only**. The deployed filename must always match the frontend fetch path:

```
Frontend calls:   /.netlify/functions/analyze
Deployed file:    netlify/functions/analyze.js   ← must always be this name
```

Version tracking lives inside the file header (`VERSIONED`, `DEPLOYED_AS` fields).

---

## Project Structure
```
niche-scorer/
├── index-v3.html                   ← frontend (current: v3 / CR004)
├── netlify.toml                    ← build + redirect + function config
├── README.md
└── netlify/
    └── functions/
        └── analyze.js              ← Anthropic API proxy (versioned: v3)
```

---

## Deploy to Netlify

### 1. Extract zip
The zip extracts with the correct folder structure — no manual moving needed.

### 2. Push to GitHub
```bash
git init
git add .
git commit -m "CR005 — netlify.toml redirect for versioned index"
git remote add origin https://github.com/YOUR_USER/niche-scorer.git
git push -u origin main
```

### 3. Connect in Netlify
- New site → Import from GitHub → select repo
- Build command: *(leave blank)*
- Publish directory: `.`

### 4. Add environment variable
In Netlify → Site settings → Environment variables:
```
ANTHROPIC_API_KEY = sk-ant-...
```

### 5. Deploy
Netlify will now serve `index-v3.html` at the root URL automatically.

---

## Local dev
```bash
npm install -g netlify-cli
netlify dev
```
Set `ANTHROPIC_API_KEY` in a `.env` file locally.

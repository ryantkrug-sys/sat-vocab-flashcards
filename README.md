# SAT Vocabulary Flashcards — GitHub Pages site

Static companion site for Ryan Krug's SAT Vocabulary Book: 1,000 words in 50
clusters (5 levels × 10 clusters), per-cluster tap-to-reveal flashcards,
5-question cluster quizzes (answers hidden until tapped), and the
Latin/Greek Building Blocks. Guess-first by design — no definition or quiz
answer is visible before the tap.

**Files**

| File | Purpose |
|---|---|
| `index.html` | The entire site (data embedded; no build step, no server) |
| `.nojekyll` | Tells GitHub Pages to serve files as-is |
| `build_site.py` | Regenerates `index.html` from the book data in `~/workspace/sat-vocab-book/data/` — only needed after data changes |

Deep links are the whole point: `https://DOMAIN/#cluster-27` must open the
page scrolled to Cluster 27 with that cluster expanded and highlighted. The
page implements this in JS (parse hash on load + on `hashchange`, clear any
filter, expand the cluster, scroll with retries while layout settles) — it
does not rely on the browser's native anchor jump. Verified 2026-11-24 in
headless Chrome with a mobile (iPhone-size) viewport: all 50 anchors land
correctly on a fresh page load.

## Deployment (GitHub Pages, account: ryantkrug-sys)

1. **Create the repo:** github.com → New repository → name it
   `sat-vocab-flashcards` → Public → Create (no README/gitignore needed).
2. **Upload the files:** "uploading an existing file" (or `git push`) —
   `index.html`, `.nojekyll`, `build_site.py`, and this README, all in the
   repo root.
3. **Turn on Pages:** repo Settings → Pages → Build and deployment →
   Source: "Deploy from a branch" → Branch: `main`, folder `/ (root)` → Save.
   The site appears at `https://ryantkrug-sys.github.io/sat-vocab-flashcards/`
   within a few minutes.
4. **Sanity-check** the github.io URL on a phone: open
   `.../sat-vocab-flashcards/#cluster-27` fresh — it must land on Cluster 27.

## Custom domain (do this before regenerating any QR codes)

1. **Buy the domain** at any registrar (Ryan's approval required first).
2. **Point DNS at GitHub Pages** at the registrar:
   - `www` → CNAME record → `ryantkrug-sys.github.io`
   - Apex (root, `@`) → four A records → `185.199.108.153`,
     `185.199.109.153`, `185.199.110.153`, `185.199.111.153`
     (or an ALIAS/ANAME record to `ryantkrug-sys.github.io` if offered)
3. **Tell the repo:** Settings → Pages → Custom domain → enter the domain
   (e.g. `www.mathwithryan.com` or the apex) → Save. This creates a `CNAME`
   file in the repo — keep it.
4. **Wait for the HTTPS checkbox:** once DNS propagates (minutes to a few
   hours), GitHub provisions a Let's Encrypt certificate; then tick
   "Enforce HTTPS".
5. **Re-test all 50 deep links** on the real domain from a phone
   (`https://DOMAIN/#cluster-1`, `#cluster-27`, `#cluster-50`, …) before
   touching the book.

## After the domain is live

- Regenerate the vocabulary book's 50 QR codes to encode
  `https://DOMAIN/#cluster-1` … `https://DOMAIN/#cluster-50` and rebuild the
  manuscript (separate task — do not edit the book before the domain works).
- Local preview anytime: `python3 -m http.server` in this folder, then open
  `http://localhost:8000/#cluster-27`.

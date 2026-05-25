# Site Redesign: Refined Slate Blue — Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Refine all pages of the Quarto personal site with a slate blue palette, system sans-serif typography, and cleaner spacing — all via CSS overrides in `styles.css`, spacelab theme untouched.

**Architecture:** Single-file CSS override strategy. All changes land in `styles.css`. The `_quarto.yml` file and all `.qmd` sources are untouched. Rollback = `git revert` on the commit that modifies `styles.css`.

**Tech Stack:** Quarto (spacelab/Bootstrap 5), plain CSS, `quarto render` CLI

---

## File Map

| File | Change |
|---|---|
| `styles.css` | All styling changes (currently empty) |
| `.gitignore` | Add `.superpowers/` entry |
| No other files touched | |

---

## Task 1: Housekeeping — gitignore + rollback baseline

**Files:**
- Modify: `.gitignore`

- [ ] **Step 1: Add `.superpowers/` to `.gitignore`**

Open `.gitignore` (create it if absent) and append:

```
.superpowers/
```

- [ ] **Step 2: Verify `styles.css` is tracked and clean**

```bash
git status styles.css
```

Expected output: `nothing to commit` (file is tracked, currently 0 meaningful bytes). This is the rollback baseline — to revert the entire redesign later, run:

```bash
git log --oneline styles.css
# find the commit hash just before the redesign changes
git show <hash>:styles.css > styles.css
quarto render
```

- [ ] **Step 3: Commit**

```bash
git add .gitignore
git commit -m "chore: ignore .superpowers/ brainstorm artifacts"
```

---

## Task 2: Global base styles — font, color, links

**Files:**
- Modify: `styles.css`

- [ ] **Step 1: Add global base block to `styles.css`**

Replace the entire (currently near-empty) contents with:

```css
/* =============================================
   GLOBAL BASE
   ============================================= */
body {
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", system-ui,
               Roboto, "Helvetica Neue", Arial, sans-serif;
  line-height: 1.65;
  color: #333;
}

h1, h2, h3, h4, h5, h6 {
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", system-ui,
               Roboto, "Helvetica Neue", Arial, sans-serif;
  color: #2c3e50;
}

a {
  color: #2980b9;
}

a:hover {
  color: #1a5276;
  text-decoration: underline;
}
```

- [ ] **Step 2: Render and verify**

```bash
quarto render index.qmd
```

Open `docs/index.html` in a browser. Verify:
- Body text uses system sans-serif (not the spacelab default serif)
- Post titles appear in `#2c3e50` (dark slate, not Bootstrap default blue)
- Link color is `#2980b9` (muted blue, not the spacelab default)

- [ ] **Step 3: Commit**

```bash
git add styles.css
git commit -m "style: add global base font, color, and link overrides"
```

---

## Task 3: Navbar overrides

**Files:**
- Modify: `styles.css`

The rendered navbar HTML uses:
- `<nav class="navbar navbar-expand-lg" data-bs-theme="dark">` — Bootstrap dark theme
- `<span class="navbar-title">` — site name
- `<a class="nav-link">` — menu items

- [ ] **Step 1: Append navbar block to `styles.css`**

```css
/* =============================================
   NAVBAR
   ============================================= */
.navbar {
  background-color: #2c3e50 !important;
  border-bottom: none !important;
  box-shadow: none !important;
}

.navbar-title {
  color: #fff !important;
  font-weight: 600;
  font-size: 15px;
}

.navbar-nav .nav-link {
  color: #bdc3c7 !important;
  font-size: 13.5px;
}

.navbar-nav .nav-link:hover,
.navbar-nav .nav-link.active {
  color: #fff !important;
}
```

- [ ] **Step 2: Render and verify**

```bash
quarto render index.qmd
```

Open `docs/index.html`. Verify:
- Navbar background is dark slate (`#2c3e50`) — not spacelab's default teal/blue
- Site name "Michael Schulte-Mecklenbeck" is white and slightly bold
- Nav links (Blog, About, CV, Publications) are `#bdc3c7` (muted), turning white on hover

- [ ] **Step 3: Commit**

```bash
git add styles.css
git commit -m "style: slate blue navbar with refined link colors"
```

---

## Task 4: Footer overrides

**Files:**
- Modify: `styles.css`

The footer HTML: `<footer class="footer"><div class="nav-footer">...</div></footer>`

- [ ] **Step 1: Append footer block to `styles.css`**

```css
/* =============================================
   FOOTER
   ============================================= */
footer.footer {
  background-color: #2c3e50;
  color: #7f8c8d;
}

.nav-footer {
  color: #7f8c8d;
  font-size: 12px;
}

.nav-footer a {
  color: #95a5a6;
}

.nav-footer a:hover {
  color: #fff;
}
```

- [ ] **Step 2: Render and verify**

```bash
quarto render index.qmd
```

Scroll to bottom of `docs/index.html`. Verify:
- Footer background matches navbar (`#2c3e50`)
- Footer text is muted gray (`#7f8c8d`)
- Page is bookended top and bottom by the same dark slate color

- [ ] **Step 3: Commit**

```bash
git add styles.css
git commit -m "style: slate blue footer to match navbar"
```

---

## Task 5: Blog listing overrides

**Files:**
- Modify: `styles.css`

Key selectors confirmed from rendered HTML:
- `.quarto-listing-category .category` — filter pills (All, Decision Making, Methods…)
- `.quarto-post` — each post row
- `.listing-title` — post title (inside an `<h3>`)
- `.listing-date`, `.metadata` — date and meta row
- `.listing-description` — description text
- `.listing-category` — per-post category tag (different from filter pills above)

- [ ] **Step 1: Append blog listing block to `styles.css`**

```css
/* =============================================
   BLOG LISTING
   ============================================= */
/* Category filter pills (sidebar: All, Decision Making…) */
.quarto-listing-category .category {
  font-size: 11px;
  text-transform: uppercase;
  letter-spacing: 0.5px;
  background: #ecf0f1;
  color: #555;
  border: none;
  border-radius: 20px;
  padding: 3px 10px;
  cursor: pointer;
  transition: background 0.15s, color 0.15s;
}

.quarto-listing-category .category:hover,
.quarto-listing-category .category.active {
  background: #2c3e50;
  color: #fff;
}

/* Post rows */
.quarto-post {
  padding: 22px 0 !important;
  border-top: 1px solid #ecf0f1 !important;
  border-bottom: none !important;
}

/* Post title */
.listing-title {
  font-size: 17px !important;
  font-weight: 600 !important;
  color: #2c3e50 !important;
  line-height: 1.35 !important;
}

/* Post date + metadata */
.listing-date,
.metadata {
  font-size: 11px !important;
  color: #7f8c8d !important;
  text-transform: uppercase;
  letter-spacing: 0.7px;
}

/* Post description */
.listing-description {
  font-size: 13.5px;
  color: #555;
  line-height: 1.6;
}

/* Per-post category tags */
.listing-category {
  font-size: 10px !important;
  text-transform: uppercase;
  letter-spacing: 0.4px;
  background: #ecf0f1 !important;
  color: #7f8c8d !important;
  border: none !important;
  border-radius: 3px !important;
  padding: 2px 8px !important;
}

.listing-category:hover {
  background: #2c3e50 !important;
  color: #fff !important;
}
```

- [ ] **Step 2: Render and verify**

```bash
quarto render index.qmd
```

Open `docs/index.html`. Verify:
- Category filter pills (All, Decision Making…) are rounded, gray background, uppercase
- Clicking "All" makes it turn dark slate with white text
- Each post row has a thin top border separator
- Post titles are `17px`, `font-weight: 600`, slate color
- Date appears uppercase and muted (e.g., "MAY 2, 2025")
- Per-post tags (Teaching, Methods…) are small gray pills, not the default Bootstrap badges

- [ ] **Step 3: Commit**

```bash
git add styles.css
git commit -m "style: refined blog listing — pills, post rows, meta typography"
```

---

## Task 6: About page overrides

**Files:**
- Modify: `styles.css`

Key selectors from rendered HTML:
- `.quarto-about-trestles` — page wrapper
- `.about-entity` — left sidebar (avatar + links)
- `.about-link` — social links
- `.about-contents h2` — section headings (Research, Teaching…)
- `.about-contents p` — body paragraphs

- [ ] **Step 1: Append about page block to `styles.css`**

```css
/* =============================================
   ABOUT PAGE (trestles template)
   ============================================= */
.quarto-about-trestles .about-entity {
  border-right: 1px solid #ecf0f1;
  padding-right: 28px;
}

.about-link {
  color: #2980b9 !important;
  font-size: 13px;
}

.about-link:hover {
  color: #1a5276 !important;
}

.about-contents h2 {
  font-size: 13px !important;
  text-transform: uppercase;
  letter-spacing: 1px;
  color: #2c3e50;
  border-bottom: 1px solid #ecf0f1;
  padding-bottom: 6px;
  margin-top: 28px;
}

.about-contents p {
  font-size: 14px;
  color: #444;
  line-height: 1.7;
}
```

- [ ] **Step 2: Render and verify**

```bash
quarto render about.qmd
```

Open `docs/about.html`. Verify:
- Sidebar (avatar + social links) has a right border separating it from the text content
- Social links (Bluesky, LinkedIn, GitHub, Scholar) appear in `#2980b9` blue
- Section headings (Research, Teaching, Recent Publications) are small, uppercase, with a thin bottom rule
- Body text is `14px` and comfortable to read

- [ ] **Step 3: Commit**

```bash
git add styles.css
git commit -m "style: about page — sidebar border, link color, section heading refinement"
```

---

## Task 7: Post detail overrides

**Files:**
- Modify: `styles.css`

Key selectors from rendered HTML:
- `.quarto-title .title` / `h1.title` — post headline
- `.quarto-title-meta` — author + date meta block
- `.quarto-title-meta-heading` — "Author", "Published" labels
- `#quarto-document-content p` — body paragraphs
- `#quarto-document-content pre` — code blocks

- [ ] **Step 1: Append post detail block to `styles.css`**

```css
/* =============================================
   POST DETAIL
   ============================================= */
.quarto-title .title {
  font-size: 28px;
  font-weight: 700;
  color: #2c3e50;
  letter-spacing: -0.4px;
  line-height: 1.25;
}

.quarto-title-meta {
  font-size: 11px;
  color: #7f8c8d;
}

.quarto-title-meta-heading {
  font-size: 10px;
  text-transform: uppercase;
  letter-spacing: 0.8px;
  color: #7f8c8d;
}

#quarto-document-content p {
  font-size: 15px;
  line-height: 1.75;
  color: #333;
}

#quarto-document-content h2 {
  font-size: 19px;
  font-weight: 700;
  color: #2c3e50;
  margin-top: 36px;
}

#quarto-document-content pre,
#quarto-document-content pre.sourceCode {
  background: #f4f6f8;
  border: 1px solid #dde3ea;
  border-radius: 4px;
}
```

- [ ] **Step 2: Render and verify**

```bash
quarto render posts/2026-04-20-scipod2026/index.qmd
```

Open `docs/posts/2026-04-20-scipod2026/index.html`. Verify:
- Post title is large (`28px`), dark slate, slightly tight tracking
- "Author" / "Published" meta labels are small and uppercase
- Body text is `15px` with `1.75` line-height — comfortable reading
- Code blocks (if present) have the light gray background and subtle border

- [ ] **Step 3: Commit**

```bash
git add styles.css
git commit -m "style: post detail — title sizing, body reading rhythm, code block polish"
```

---

## Task 8: Publications page overrides

**Files:**
- Modify: `styles.css`

The publications page uses Bootstrap's `list-group` generated by the Python script in `research.qmd`. Key selectors:
- `.list-group-item` — each publication row
- `.btn-outline-dark` — "Paper" / PDF buttons on each entry

- [ ] **Step 1: Append publications block to `styles.css`**

```css
/* =============================================
   PUBLICATIONS
   ============================================= */
.list-group-item {
  border-color: #ecf0f1;
  padding: 16px 0;
  font-size: 14px;
  line-height: 1.6;
  color: #444;
}

.btn-outline-dark {
  color: #2980b9 !important;
  border-color: #2980b9 !important;
  font-size: 11px;
  font-weight: 500;
}

.btn-outline-dark:hover {
  background-color: #2980b9 !important;
  color: #fff !important;
  border-color: #2980b9 !important;
}
```

- [ ] **Step 2: Render and verify**

```bash
quarto render research.qmd
```

Open `docs/research.html`. Verify:
- Publication rows are separated by light `#ecf0f1` borders
- Publication text is `14px` and readable
- "Paper" buttons are outlined in `#2980b9` and fill with the same blue on hover (not the default dark button)

- [ ] **Step 3: Commit**

```bash
git add styles.css
git commit -m "style: publications — row borders and slate-blue action buttons"
```

---

## Task 9: Full render + visual QA across all pages

**Files:** None changed — read-only verification step.

- [ ] **Step 1: Render the full site**

```bash
quarto render
```

Expected: No errors. Output in `docs/`.

- [ ] **Step 2: Visual check — Blog listing (`docs/index.html`)**

Open in browser. Check:
- Navbar: dark slate, white brand, muted links
- Category pills: rounded, gray, uppercase
- Post rows: separated by thin lines, good vertical breathing room
- Post titles: slate, 17px, bold
- Post meta: uppercase, muted
- Footer: dark slate, matches navbar

- [ ] **Step 3: Visual check — About (`docs/about.html`)**

Check:
- Sidebar visible, separated from text by a right border
- Avatar photo displays correctly (no change from before)
- Social links: blue, properly sized
- Section headings: uppercase, with bottom rule
- Body paragraphs: readable, 14px, good line-height

- [ ] **Step 4: Visual check — Publications (`docs/research.html`)**

Check:
- Publication entries readable, well-spaced
- "Paper" buttons use `#2980b9` blue outline, fill on hover
- Year headings styled correctly

- [ ] **Step 5: Visual check — One post detail**

Open any post, e.g. `docs/posts/2026-04-20-scipod2026/index.html`. Check:
- Title large and dark
- Body comfortable to read
- No layout breakage

- [ ] **Step 6: Check mobile responsiveness**

In Chrome DevTools (F12 → Toggle Device Toolbar), test at 375px width. Verify:
- Navbar collapses correctly
- Text doesn't overflow
- Blog listing is readable on small screens

---

## Task 10: Push to GitHub

- [ ] **Step 1: Confirm clean state**

```bash
git status
```

Expected: working tree clean (all changes committed in Tasks 1-8).

- [ ] **Step 2: Push to origin**

```bash
git push origin main
```

Netlify auto-deploys from `main` — the live site at `schulte-mecklenbeck.com` will update within 1-2 minutes.

- [ ] **Step 3: Verify live site**

Open `https://www.schulte-mecklenbeck.com` in a browser and confirm the slate blue navbar and refined typography are live.

---

## Rollback Reference

To undo the entire redesign at any point:

```bash
# Find the commit hash just before Task 2 began
git log --oneline styles.css

# Restore the old (empty) styles.css
git show <pre-redesign-hash>:styles.css > styles.css

# Render to apply
quarto render

# Commit the revert
git add styles.css
git commit -m "revert: restore original styles.css"
git push origin main
```

The spec lives at `docs/superpowers/specs/2026-05-25-site-redesign-design.md` for future reference.

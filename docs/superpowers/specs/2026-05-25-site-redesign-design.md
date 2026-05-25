# Site Redesign: Refined Slate Blue — Design Spec

**Date:** 2026-05-25  
**Scope:** All pages — navbar, blog listing, about, publications, post detail  
**Approach:** CSS overrides only (`styles.css`); spacelab theme stays in `_quarto.yml`

---

## Goals

Refine the existing spacelab-based site toward the clean, minimal aesthetic of ai-2027.com — without a structural overhaul. Every change lives in `styles.css` so rollback is a single `git revert`.

---

## Design Decisions

| Dimension | Decision |
|---|---|
| Base theme | spacelab (unchanged in `_quarto.yml`) |
| Navbar background | `#2c3e50` (dark slate) |
| Navbar brand/links | white brand, `#bdc3c7` links → white on hover |
| Body font | `-apple-system, BlinkMacSystemFont, "Segoe UI", system-ui, sans-serif` |
| Body line-height | `1.65` |
| Heading color | `#2c3e50` |
| Body text color | `#333` |
| Link color | `#2980b9` |
| Meta / secondary text | `#7f8c8d` |
| Borders / dividers | `#ecf0f1` |
| Published button | `#2980b9` background, white text |
| Working paper button | `#ecf0f1` background, `#555` text |

---

## Page-by-Page Spec

### Navbar (all pages)

- Background: `#2c3e50`
- Brand text: white, `font-weight: 600`
- Nav links: `#bdc3c7`, hover → `#fff`, no underline
- Height: ~52px with vertical centering
- No border-bottom

### Blog Listing (`index.qmd`)

- Category pills: rounded, `#ecf0f1` background, uppercase 11px; active pill uses `#2c3e50` bg + white text
- Post items: separated by `1px solid #ecf0f1` top/bottom borders, 22px vertical padding
- Post date: uppercase, `11px`, `#7f8c8d`, `letter-spacing: 0.7px`
- Post title: `17px`, `font-weight: 600`, `#2c3e50`, `line-height: 1.35`
- Post description: `13.5px`, `#555`, `line-height: 1.6`
- Post category tags: `10px`, `#ecf0f1` bg, `#7f8c8d` text, `border-radius: 3px`
- Max content width: `760px`, centered

### About Page (`about.qmd`)

- Layout: two-column (sidebar + main) via CSS on the trestles template
- Sidebar background: `#fafafa`, `border-right: 1px solid #ecf0f1`
- Avatar image: existing `avatar.jpg`, displayed as-is (no change)
- Name: `15px`, `font-weight: 700`, `#2c3e50`
- Role text: `12px`, `#7f8c8d`
- Social links: `#2980b9`, `12px`
- Section headings in main: `14px`, uppercase, `letter-spacing: 1px`, `#2c3e50`, with `1px solid #ecf0f1` bottom border
- Body text: `14px`, `#444`, `line-height: 1.7`

### Publications (`research.qmd`)

- Section titles (Published / Working Papers): `11px`, uppercase, `letter-spacing: 1.5px`, `#7f8c8d`, with `2px solid #2c3e50` bottom border
- Publication items: grid layout (title/authors/venue left, button right), `1px solid #ecf0f1` bottom border, `18px` vertical padding
- Title: `14.5px`, `font-weight: 600`, `#2c3e50`
- Authors: `12px`, `#7f8c8d`
- Venue: `12px`, `#555`, italic
- Published button: `#2980b9` bg, white text, `border-radius: 3px`, `11px`
- Working paper button: `#ecf0f1` bg, `#555` text
- Max content width: `820px`, centered

### Post Detail (individual posts)

- Max content width: `680px`, centered
- Date/category meta: `11px`, uppercase, `#7f8c8d`, `letter-spacing: 0.8px`
- Post title: `28px`, `font-weight: 700`, `#2c3e50`, `letter-spacing: -0.4px`, `line-height: 1.25`
- Divider below title: `1px solid #ecf0f1`
- Body paragraphs: `15px`, `line-height: 1.75`, `#333`
- In-post headings (h2): `19px`, `font-weight: 700`, `#2c3e50`, `36px` top margin
- Code blocks: `#f4f6f8` bg, `1px solid #dde3ea` border, `border-radius: 4px`, `13px` monospace

### Footer (all pages)

- Background: `#2c3e50` (matches navbar — bookends the page)
- Text: `#7f8c8d`, `11px`, centered

---

## Rollback

All changes are confined to `styles.css`. To revert:

```bash
git log --oneline styles.css   # find the commit before the redesign
git show <hash>:styles.css > styles.css  # restore old version
# or simply:
git revert <redesign-commit-hash>
```

The `_quarto.yml` theme (`spacelab`) is untouched, so even if `styles.css` is blanked the site renders correctly — just without the refinements.

---

## Out of Scope

- No changes to `_quarto.yml` structure
- No new Quarto extensions or plugins
- No JavaScript additions
- No changes to post content or `papers.yaml`
- No changes to image files

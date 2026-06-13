---
name: update-weekly-papers
description: >-
  Add weekly English and Chinese arXiv pick PDFs to the Awesome Physics of AI
  site: rename files in docs/public/recent-papers/, update docs/recent-papers.md
  table (newest week first). Use when the user says 更新这周Paper, 更新本周论文,
  加这周PDF, update weekly papers, add this week's papers, or adds new PDFs to
  docs/public/recent-papers/.
---

# Update Weekly Papers

Weekly workflow for the VitePress site in this repo.

## What the user does

Before invoking this skill, the user drops **two PDFs** into `docs/public/recent-papers/`:

- English: e.g. `Weekly arXiv Picks - June 13, 2026.pdf`
- Chinese: e.g. `每周 arXiv 好文推荐｜2026-06-13.pdf`

Filenames may vary; extract the date from them.

## Agent checklist

```
Task Progress:
- [ ] Step 1: Find the two new PDFs in docs/public/recent-papers/
- [ ] Step 2: Determine the week date (YYYY-MM-DD)
- [ ] Step 3: Rename PDFs to canonical names
- [ ] Step 4: Add a table row to docs/recent-papers.md (top of table)
- [ ] Step 5: Verify files and links
```

## Step 1: Find new PDFs

List `docs/public/recent-papers/`. New files are usually:

- Untracked in git, or
- Not matching `YYYY-MM-DD-en.pdf` / `YYYY-MM-DD-zh.pdf`

There should be exactly one English and one Chinese PDF for the new week.

## Step 2: Determine the week date

Priority:

1. Date in the Chinese filename (`2026-06-13` in `每周 arXiv 好文推荐｜2026-06-13.pdf`)
2. Date parsed from the English filename (`June 13, 2026` → `2026-06-13`)
3. Ask the user if ambiguous

Use `YYYY-MM-DD` everywhere (table, filenames, link text).

## Step 3: Rename PDFs

Rename in `docs/public/recent-papers/`:

| Language | Canonical filename |
| -------- | ------------------ |
| English  | `YYYY-MM-DD-en.pdf` |
| Chinese  | `YYYY-MM-DD-zh.pdf` |

Example:

```bash
mv "Weekly arXiv Picks - June 13, 2026.pdf" "2026-06-13-en.pdf"
mv "每周 arXiv 好文推荐｜2026-06-13.pdf" "2026-06-13-zh.pdf"
```

Do not rename or delete existing weeks' PDFs.

## Step 4: Update docs/recent-papers.md

Edit only the markdown table (keep frontmatter and `<style>` block unchanged).

**Insert the new row immediately below the header row.** Newest date on top.

Row template:

```markdown
| Week of YYYY-MM-DD | [Weekly arXiv Picks — Month D, YYYY](/recent-papers/YYYY-MM-DD-en.pdf) | [每周 arXiv 好文推荐｜YYYY-MM-DD](/recent-papers/YYYY-MM-DD-zh.pdf) |
```

Formatting rules:

- English link text: `Weekly arXiv Picks — Month D, YYYY` (em dash `—`, not hyphen `-`)
- Chinese link text: `每周 arXiv 好文推荐｜YYYY-MM-DD` (fullwidth pipe `｜`)
- PDF paths: `/recent-papers/YYYY-MM-DD-en.pdf` and `/recent-papers/YYYY-MM-DD-zh.pdf`
- Sort all rows by date descending (most recent first)

Example after adding 2026-06-20:

```markdown
| Week | English | 中文 |
| ---- | ------- | ---- |
| Week of 2026-06-20 | [Weekly arXiv Picks — June 20, 2026](/recent-papers/2026-06-20-en.pdf) | [每周 arXiv 好文推荐｜2026-06-20](/recent-papers/2026-06-20-zh.pdf) |
| Week of 2026-06-13 | [Weekly arXiv Picks — June 13, 2026](/recent-papers/2026-06-13-en.pdf) | [每周 arXiv 好文推荐｜2026-06-13](/recent-papers/2026-06-13-zh.pdf) |
| Week of 2026-06-06 | [Weekly arXiv Picks — June 6, 2026](/recent-papers/2026-06-06-en.pdf) | [每周 arXiv 好文推荐｜2026-06-06](/recent-papers/2026-06-06-zh.pdf) |
```

If the week already exists in the table, update links only — do not duplicate the row.

## Step 5: Verify

1. `ls docs/public/recent-papers/` — both `YYYY-MM-DD-en.pdf` and `YYYY-MM-DD-zh.pdf` exist
2. Table is sorted newest-first with no duplicate dates
3. Link paths match renamed files

Optional: remind user to preview at `http://localhost:5173/awesome-physics-of-ai/recent-papers` (`npm run docs:dev`).

## Do not

- Commit unless the user explicitly asks
- Rebuild `docs/.vitepress/dist/` unless asked (dev server serves from `docs/public/`)
- Change `docs/.vitepress/config.mts` or other pages
- Modify unrelated files

## Edge cases

- **Only one PDF present**: ask which language is missing; do not proceed with half a week
- **Date mismatch between EN/ZH files**: ask the user which date is correct
- **PDFs already correctly named**: skip rename; only update the table if needed

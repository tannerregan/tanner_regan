# Project Instructions

This repo is a static GitHub Pages site (HTML/CSS, no build system). The published
pages are `index.html`, `research.html`, `policy.html`, `events.html`, `teaching.html`,
plus `assets/`.

## Site structure

- **Masthead** (`masthead` / `masthead__inner-wrap`): top bar with site title and nav links. Nav contains: Home, Research, Policy, Events, Teaching. There is no CV link in the nav — the CV is accessed via the Download CV button on the home page.
- **Sidebar** (`aside.sidebar`): shown on all pages; contains profile photo, name, bio, email, social icons (Google Scholar, X, LinkedIn, Bluesky, RePEc).
- **`index.html` only**: a `research-interests` box appears at the top of `main.page__content`, containing the field label, topic keywords, and a "Download CV" pill button (`a.cv-download-btn`) that links to `CV/Tanner_Regan_CV.pdf` and opens in a new tab.
- **Stylesheet**: `assets/css/style.css`. Uses GW brand colors defined as CSS variables: `--gw-navy: #00223e`, `--gw-blue: #033c5a`, `--gw-buff: #d6bf91`, `--gw-buff-light: #f6f1e8`.

## PDF link convention

Every link to a PDF (local or external) must include `target="_blank" rel="noopener"` so the file opens in a new tab. Apply this whenever adding or editing PDF links.

## Keeping the repo public-facing only

`private/` is a local-only folder (listed in `.gitignore`) for files that should
stay on this machine and never be pushed to GitHub or served on the site.

Whenever files in `CV/`, `events/`, `papers/`, or `teaching/` are added, removed,
or the HTML pages are edited, check for mismatches and fix them:

- **Unreferenced -> private**: Any PDF (or other file) in `CV/`, `events/`,
  `papers/`, or `teaching/` that is NOT linked from `index.html`, `research.html`,
  `policy.html`, `events.html`, or `teaching.html` should be moved into the matching
  subfolder under `private/` (e.g. `papers/foo.pdf` -> `private/papers/foo.pdf`),
  and removed from git tracking (`git rm`).
- **Referenced -> back out of private**: If a file inside `private/` becomes
  referenced by a link in one of those HTML pages, move it out of `private/` back
  to its corresponding public folder so the link resolves.

After moving files, verify every local href in the five HTML pages resolves to an
existing file (no broken links), e.g.:

```bash
for f in index.html research.html policy.html events.html teaching.html; do
  grep -oE 'href="[^"]+"' "$f" | sed -E 's/href="([^"]*)"/\1/' | while read -r link; do
    case "$link" in http*|mailto:*) continue ;; esac
    [ -f "$link" ] || echo "MISSING: $f -> $link"
  done
done
```

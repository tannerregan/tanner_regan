# Project Instructions

This repo is a static GitHub Pages site (HTML/CSS, no build system). The published
pages are `index.html`, `research.html`, `events.html`, `teaching.html`, plus
`assets/`.

## Keeping the repo public-facing only

`private/` is a local-only folder (listed in `.gitignore`) for files that should
stay on this machine and never be pushed to GitHub or served on the site.

Whenever files in `CV/`, `events/`, `papers/`, or `teaching/` are added, removed,
or the HTML pages are edited, check for mismatches and fix them:

- **Unreferenced -> private**: Any PDF (or other file) in `CV/`, `events/`,
  `papers/`, or `teaching/` that is NOT linked from `index.html`, `research.html`,
  `events.html`, or `teaching.html` should be moved into the matching subfolder
  under `private/` (e.g. `papers/foo.pdf` -> `private/papers/foo.pdf`), and removed
  from git tracking (`git rm`).
- **Referenced -> back out of private**: If a file inside `private/` becomes
  referenced by a link in one of those HTML pages, move it out of `private/` back
  to its corresponding public folder so the link resolves.

After moving files, verify every local href in the four HTML pages resolves to an
existing file (no broken links), e.g.:

```bash
for f in index.html research.html events.html teaching.html; do
  grep -oE 'href="[^"]+"' "$f" | sed -E 's/href="([^"]*)"/\1/' | while read -r link; do
    case "$link" in http*|mailto:*) continue ;; esac
    [ -f "$link" ] || echo "MISSING: $f -> $link"
  done
done
```

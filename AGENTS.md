# AGENTS

## Project shape (what to edit)
- This repo is a **single Quarto website** (no package/monorepo): pages are root-level `*.qmd`, site config is `_quarto.yml`, styling is `styles.css`, media lives in `assets/`.
- Rendered output goes to `_site/` (`project.output-dir` in `_quarto.yml`) and is generated artifact; do not hand-edit it.

## Setup and environment
- R environment is `renv`-managed. `.Rprofile` auto-sources `renv/activate.R`, so run commands from repo root to use the project library.
- CI uses **R 4.4.1** (`.github/workflows/quarto-publish.yml`); keep lockfile-compatible changes (`renv.lock`) when adding/changing R deps.
- CI installs many system libs (GDAL/ImageMagick/font libs). If local render fails on compiled packages, match CI system deps first.

## Canonical commands
- Restore dependencies: `Rscript -e 'renv::restore(confirm = FALSE)'`
- Render site: `quarto render`
- Live preview: `quarto preview`

## Verification expectations
- There is no dedicated test/lint pipeline; the practical verification is a clean `quarto render`.
- CI trigger is push/PR to `main`; workflow renders then publishes `_site/` to GitHub Pages.

## Repo-specific gotchas
- `index.qmd` geocodes institutions at render time via `nominatimlite::geo_lite(...)`; rendering depends on network access and can fail/slow due to external geocoding service behavior.
- `README.md` is encoded as UTF-16LE with CRLF. Preserve encoding or convert intentionally when editing.

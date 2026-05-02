# AGENTS

## Project shape (what to edit)
- Single Quarto website (no package/monorepo): pages are root-level `*.qmd`, site config is `_quarto.yml`, styling is `styles.css`, media in `assets/`.
- Rendered output goes to `_site/` (generated artifact; do not hand-edit).

## Setup and environment
- R environment is `renv`-managed. `renv/activate.R` is tracked, but `.Rprofile` (which sources it) is **gitignored** — new clones must bootstrap renv before rendering.
- CI uses **R 4.4.1** (`.github/workflows/quarto-publish.yml`); keep lockfile-compatible changes (`renv.lock`) when adding/changing R deps.
- CI installs many system libs (GDAL/ImageMagick/font libs). If local render fails on compiled packages, match CI system deps first.

## Canonical commands
- Bootstrap renv (new clone): `Rscript -e 'renv::activate(); renv::restore(confirm = FALSE)'`
- Restore dependencies (existing setup): `Rscript -e 'renv::restore(confirm = FALSE)'`
- Render site: `quarto render`
- Live preview: `quarto preview`

## Verification
- No dedicated test/lint pipeline; a clean `quarto render` is the practical verification.
- CI triggers on push/PR to `main`; workflow renders then publishes `_site/` to GitHub Pages.

## Gotchas
- `index.qmd` geocodes institutions at render time via `nominatimlite::geo_lite(...)` — rendering requires network access and can fail/slow due to external geocoding service behavior.
- `README.md` is UTF-16LE with CRLF. Preserve encoding or convert intentionally when editing.

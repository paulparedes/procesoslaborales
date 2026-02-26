# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

```bash
# Preview the site locally (with hot reload)
quarto preview

# Full render to _site/
quarto render

# Render a single file
quarto render index.qmd
```

## Project architecture

This is a **Quarto website** (`_quarto.yml`, `project.type: website`) for the course *Procesos Laborales* taught by Paul Paredes at PUCP and UNMSM graduate programs.

### Key files

| File | Role |
|------|------|
| `_quarto.yml` | Global config: navbar, fonts, SEO, footer, Google Analytics |
| `index.qmd` | Homepage — hero section, RA grid, unit cards, schedule table, professor bio |
| `paulparedes-quarto.scss` | All custom styles; defines the full design system |
| `lectures-YYYY-S.qmd` | Listing page (grid) for each semester's lectures |
| `archivo.qmd` | Historical archive — **do not delete** |

### Content structure per semester

Each semester lives in its own folder `YYYY-S/` containing `lecture1.qmd` … `lectureN.qmd`. The listing page `lectures-YYYY-S.qmd` points to `"YYYY-S/lecture*.qmd"` via Quarto's `listing` feature.

**Lecture front matter pattern:**
```yaml
---
title: "N | Título de la clase"
date: "YYYY-MM-DD"
author: "Paul Paredes"
description: "Clase N"
image: "/images/lectureN.png"   # use .jpg for some; check /images/
date-format: long
lang: es
format: html
---
```

**Listing page pattern** (copy `lectures-2026-1.qmd` as template):
```yaml
listing:
- id: lectures
  contents: "YYYY-S/lecture*.qmd"
  sort: "date asc"
  type: grid
  fields: [image, date, title, author]
```

### Presentations (xaringan/revealjs)

Rendered HTML slides live in `xaringan/`. Lecture pages in older semesters link to them directly. The `2025-1/template.qmd` shows the revealjs format used for in-class slides (`custom.css` handles slide styles).

### Design system (`paulparedes-quarto.scss`)

Color palette variables: `$pp-primary` (#1E3A5F), `$pp-accent` (#C49332), `$pp-secondary` (#3D7A8A), `$pp-contrast` (#9B3B2D), `$pp-bg` (#F7F5F0), `$pp-bg-alt` (#EDEAE4).

Typography: `Merriweather` (body + headings) + `Fira Sans` (UI/navbar) + `Fira Mono` (code). Fonts are loaded via Google Fonts in `_quarto.yml` → `include-in-header`.

Custom component classes: `.course-hero`, `.course-meta-badge`, `.unit-card`, `.unit-grid`, `.ra-card`, `.ra-grid`.

### Adding a new semester

1. Create `YYYY-S/` folder with `lectureN.qmd` files
2. Create `lectures-YYYY-S.qmd` (copy existing listing page)
3. Update `_quarto.yml` navbar: move current semester to "Archivo" dropdown, add new semester as primary left nav item
4. Update `index.qmd`: hero badge year, callout text, cronograma table, evaluación

### SEO / metadata

`index.qmd` contains a `<script type="application/ld+json">` block with `schema.org/Course` structured data. Update it when the semester changes. The `_quarto.yml` `open-graph.description` and `website.description` should also be kept current.

### Deployment

The site deploys automatically via **Netlify** on push to `main`. The rendered output goes to `_site/` (not committed). The canonical URL is `https://procesoslaborales.paulparedes.pe`.

### Files to ignore

`notas-quarto.txt` — scratch notes from a different course (residual). `index.qmd.old`, `_quarto.yml.old` — backup snapshots. `modules/`, `data/` — residual from the original template (unused). `styles.css`, `custom.css` — `custom.css` is used by revealjs slides; `styles.css` is a residual.

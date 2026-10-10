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

This is a **Quarto website** (`_quarto.yml`, `project.type: website`) for the course *Procesos Laborales* taught by Paul Paredes in the Maestría en Derecho del Trabajo y de la Seguridad Social (PUCP) and the Maestrías en Derecho Procesal (PUCP and UNMSM). Not every programme runs every semester (2026-II: only Derecho Procesal PUCP, DPR623, two modules).

### Key files

| File | Role |
|------|------|
| `_quarto.yml` | Global config: navbar, fonts, SEO, footer, Google Analytics |
| `index.qmd` | Homepage — generic hero, note, `{{< include cursos/_YYYY-S.qmd >}}` of the current semester, professor bio |
| `cursos/_YYYY-S.qmd` | Semester sheet (partial, **no YAML**, root-absolute links `/...`): badges, RA, units/modules, schedule, evaluation |
| `cursos/YYYY-S.qmd` | Thin page (front matter only) that includes the partial; permanent URL of the sheet |
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

1. Create `cursos/_YYYY-S.qmd` (partial) and `cursos/YYYY-S.qmd` (wrapper; copy `cursos/2026-2.qmd`)
2. Create `lectures-YYYY-S.qmd`: either a listing over `YYYY-S/lecture*.qmd` (template `lectures-2026-1.qmd`) or per-module session tables (template `lectures-2026-2.qmd`)
3. Update `index.qmd`: change the `{{< include cursos/_YYYY-S.qmd >}}` line, hero button, callout text, description/keywords and JSON-LD
4. Update `_quarto.yml` navbar: new semester as primary left item; move the previous one (sheet + classes) into the "Cursos anteriores" menu; update the "Descargas" Drive links
5. Never rename or delete existing `lectures-*.qmd`, `YYYY-S/` folders or `cursos/*.qmd` — their URLs must keep working

### SEO / metadata

`index.qmd` contains a `<script type="application/ld+json">` block with `schema.org/Course` structured data. Update it when the semester changes. The `_quarto.yml` `open-graph.description` and `website.description` should also be kept current.

### Deployment

The site deploys via **Netlify** on push to `main`. There is no build config in the repo: the rendered output in `_site/` **is committed**, so run `quarto render` and commit `_site/` together with the sources. The canonical URL is `https://procesoslaborales.paulparedes.pe`.

### Logo (`images/logo.png`)

El logo del sitio es `images/logo.png`, referenciado en `index.qmd` con `width="220"`. El diseño del logo es responsabilidad del usuario — **no intentar reemplazarlo ni generarlo desde Claude Code**.

### Lecture pages (`YYYY-S/lectureN.qmd`)

Estructura de cada archivo de clase (basada en el sílabo TSS603 2026-1):

```markdown
[→ Diapositivas](../xaringan/RUTA.html){.btn-hero-primary target="_blank"}

## Contenidos de la sesión
## Dinámica de clase
## Evaluación
## Lecturas obligatorias
## Lecturas complementarias   ← solo si las hay
```

Las sesiones 10 (examen parcial) y 16 (examen final) no llevan botón de diapositivas.
Los paths de diapositivas siguen la convención `xaringan/2026-NN-tema.html`.

### Files to ignore / residuales

- `notas-quarto.txt` — apuntes de otro curso (residual)
- `index.qmd.old`, `_quarto.yml.old` — snapshots de respaldo
- `custom.css` — usado por slides revealjs; no tocar
- `_para_archivar/` — carpeta vacía; conservar por si se reutiliza
- `slides-2024-1.zip`, `README.html`, `README.md` — archivos residuales del clon original

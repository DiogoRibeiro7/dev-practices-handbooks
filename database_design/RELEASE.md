# Release Guide — Relational Database Design (Concept-First)

## Build
From the repository root:

```bash
cd database_design
latexmk -pdf -interaction=nonstopmode -halt-on-error book/main.tex
```

The output PDF is generated at:

```
database_design/build/main.pdf
```

## Versioning PDFs
- Use semantic versioning for releases: `MAJOR.MINOR.PATCH`.
- Increment:
  - MAJOR when the structure or chapter order changes.
  - MINOR when new chapters or substantial content are added.
  - PATCH for edits, fixes, or small improvements.

## Naming Convention for Releases
- Tag name: `db-design-vMAJOR.MINOR.PATCH`
- PDF name: `db-design-vMAJOR.MINOR.PATCH.pdf`
- Example:
  - Tag: `db-design-v1.2.0`
  - PDF: `db-design-v1.2.0.pdf`

## Suggested Release Steps
1) Build locally and confirm the PDF opens correctly.
2) Copy `database_design/build/main.pdf` to the release name.
3) Create a git tag with the version.
4) Attach the versioned PDF to the release.

# Consistency Notes
Date: 2026-02-02

## Chosen third domain
- University (consistent mini-domain across chapters)
  - Canonical entities: Student, Course, Enrollment, Department, Instructor

## Notation rules
- Relations: `\rel{Name}(...)` in book notation.
- Primary keys: underline with `\underline{...}`.
- Foreign keys: arrow `attribute \rightarrow \rel{Other}`.
- Optional attributes: `?` only when optionality matters.
- No SQL, indexing, query optimization, or transactions/isolation.

## Macros and environments
- `\term{}` italicizes terms.
- `\rel{}` renders relation names in **bold** (per `styles/book.sty`).
- Box environments: `learningobjectives`, `definitionbox`, `examplebox`, `principlebox`, `notebox`, `chaptersummary`, `exercisebox`.

## Naming conventions
- Relation names: singular nouns (`\rel{Patient}`, `\rel{Loan}`).
- Primary keys: `<entity>_id` (e.g., `patient_id`).
- Foreign keys: same naming as referenced entity (`doctor_id` → `\rel{Doctor}`).
- Status attributes: use constrained vocab (e.g., `status ∈ {scheduled, cancelled}`).
- Dates: include semantic moment (`checkout_date`, `return_date`, `created_at`).

## Style guidelines (for expansions)
- Sober tone: short sentences mixed with longer ones.
- Avoid long bullet-only sections; use short paragraphs.
- Add one “Common mistakes” section and one “Mini checklist” section per chapter.
- Use clinic + library examples primarily, plus university examples as the third domain.

# Roadmap: Relational Database Design (Concept-First)

## Inventory
- Entrypoint: `database_design/main.tex`
- Chapters in `database_design/chapters/` (TeX sources):
  - `ch01-workflow.tex`
  - `ch02-core-concepts.tex`
  - `ch03-keys-identity.tex` (currently empty)
  - `ch04-conceptual-to-relational.tex`
  - `ch05-normalization.tex`
  - `ch06-integrity-constraints.tex`
  - `ch07-validation.tex`
  - `ch08-naming-documentation.tex`
  - `ch09-case-study-clinic.tex`
  - `ch10-case-study-library.tex`
  - `ch11-state-and-time.tex`
  - `ch12-relationships-advanced.tex`
  - `ch13-functional-dependencies.tex`
  - `ch14-normalization-patterns.tex`
  - `ch15-design-tradeoffs.tex`
  - `ch16-conceptual-design-review.tex`
- Not included in `main.tex` but present in `chapters/` (likely leftovers):
  - `ch11-advanced-topics.aux` (no .tex)
  - `ch12-advanced-topics.aux` (no .tex)
  - `ch13-functionunctional-dependencies.aux` (typo)
  - `ch14-summary-best-practices.aux` (no .tex)
  - `ch15-design-process.aux` (no .tex)
  - `ch16-summary-best-practices.aux` (no .tex)

## Custom environments and macros
Defined in `database_design/main.tex`:
- Commands: `\term{}`, `\rel{}`
- ER helpers: `\erentity`, `\errel`, `\erconnect`, plus TikZ styles `er line`, `er card`
- tcolorbox environments:
  - `learningobjectives`, `definitionbox`, `principlebox`, `examplebox`, `notebox`, `deliverablebox`, `exercisebox`, `chaptersummary`

## Packages in use
From `database_design/main.tex`:
- Encoding/fonts: `inputenc`, `fontenc`, `lmodern`, `microtype`, `parskip`
- Layout: `geometry`
- Math/tables/lists: `amsmath`, `amssymb`, `booktabs`, `tabularx`, `enumitem`
- Links: `hyperref`
- Diagrams: `tikz` (libraries: `arrows.meta`, `positioning`, `shapes.geometric`, `calc`, plus repeated `positioning, shapes, arrows`)
- Boxes: `tcolorbox` (`breakable`, `skins`)

## Current compilation status
- Build completes with warnings (overfull/underfull hboxes, duplicate destination warnings); no fatal errors in the latest `main.log`.
- Known content blocker: `ch03-keys-identity.tex` is empty (chapter shell missing).

---

## Missing chapters (proposed, concept-first, no SQL)

### 1) Keys and Identity (fill `ch03-keys-identity.tex`)
- What identity means in a conceptual model
- Natural vs surrogate keys: tradeoffs and stability
- Composite keys and when they’re meaningful
- Uniqueness vs identifiers vs labels
- Identity in associations and events
- Key selection checklist

### 2) Attributes, Domains, and Value Semantics
- Attribute meaning vs storage convenience
- Domain constraints and controlled vocabularies
- Derived vs stored attributes (conceptual rule)
- Nullability as a semantic choice
- Units, precision, and time semantics
- Attribute naming and definition format

### 3) Relationship Semantics Beyond Cardinality
- Mandatory/optional nuance and exceptions
- Role names and directionality
- Symmetric/recursive relationships
- Relationship attributes and why they matter
- Identifying vs non-identifying relationships
- When a relationship becomes an entity

### 4) Modeling Processes and State Over Time (expands ch11)
- Events vs states vs histories
- Time windows and effective dating
- Current vs historical truth
- Lifecycle states and transitions
- Temporal constraints and invariants
- Examples without SQL (concept-only)

### 5) Design Quality and Review
- Consistency checks for concepts and rules
- Anti-patterns and common failure modes
- Model walkthrough protocol
- Review artifacts: rule list, glossary, diagram
- Validation with stakeholders
- Minimal acceptance checklist

### 6) Summary and Best Practices (capstone)
- Core principles recap
- High-impact heuristics
- Typical pitfalls and how to avoid them
- What to document for handoff
- Quick-start checklist for new models
- Suggested practice exercises

---

## Missing figures / diagrams
- Chapter-level ER diagrams for each domain example (clinic, library, plus at least one new domain)
- Conceptual-to-relational mapping schematic (entities → relations + keys)
- Relationship taxonomy diagram (1–1, 1–N, N–N, identifying)
- Functional dependency graphs for normalization chapters
- Decomposition flow for normalization patterns
- Timeline/state diagrams for the state-and-time chapter
- Review checklist flow diagram (model validation steps)

---

## Missing frontmatter / backmatter
- Frontmatter:
  - Expanded preface (audience + scope + how to use the book)
  - Acknowledgements (optional)
- Backmatter:
  - Bibliography stub (for modelling/DB design references)
  - Glossary (terms like entity, attribute, constraint, FD, etc.)
  - Index (optional; depends on final scope)
  - List of figures/tables (optional, if figures expand)

---

## Build steps
- Quick build:
  - `pdflatex main.tex`
  - `pdflatex main.tex` (second pass for TOC/bookmarks)
- Full build (if latexmk is available):
  - `latexmk -pdf -interaction=nonstopmode main.tex`
- Clean (manual): delete `*.aux`, `*.out`, `*.toc`, `*.lof`, `*.lot`, and `chapters/*.aux`

# Contributing Guide

Thank you for improving the Dev Practices Handbooks. Follow this process so contributions stay consistent and buildable.

## Chapters

1. Review `docs/CHAPTER_TEMPLATE.md` and `docs/STYLE_GUIDE.md` for tone, structure, and formatting rules.
2. Create or expand a chapter under the relevant handbook (`software-testing-handbook/chapters/`, etc.). Keep the template outline, add TODO markers where needed, and finish with learning goals, examples, checklists, and exercises.
3. Add deterministic examples under `*/examples/chXX/` (e.g., `software-testing-handbook/examples/ch07`). Provide a `README.md` describing how to run the example locally and in CI. Seed randomness, freeze time, and isolate I/O.
4. If you add a new handbook chapter, update `docs/NEW_CHAPTERS.md` and the table of contents in the handbook’s `main.tex` to include it.

## Examples & Automation

- Lint markdown with `npm run lint` (requires `node` 18+).
- Spell-check markdown with `npm run lint:spell` (uses `cspell` configuration at the repo root).
- Keep Python example code formatted with `black --check <path>` and linted by `ruff check <path>`.
- Link-check markdown with `lychee --config lychee.toml`.
- Build the documentation site with `mdbook build docs/book` and verify `docs/BUILD.md` for detailed commands.
- Build LaTeX handbooks using `latexmk -pdf main.tex` inside each handbook directory (e.g., `software-testing-handbook`).

## Pull Requests

- Run all checks locally before pushing (see `docs/BUILD.md`).
- Include the generated PDF artifacts or mention how to produce them.
- Update `docs/book/src/new-chapters.md` if you introduce new topics.
- Ensure any new `*.md` files pass `markdownlint` and `cspell`.

## Testing & CI

CI executes the above checks on every pull request. Fix any detected issues locally before merging to keep the main branch green.

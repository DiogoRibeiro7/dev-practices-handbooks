# Build & Checks (Summary)

Detailed instructions live in `docs/BUILD.md`, but here are the key commands used by CI:

1. `mdbook build docs/book`
2. `npm run lint`
3. `lychee --config lychee.toml`
4. `ruff check software-testing-handbook/examples`
5. `black --check software-testing-handbook/examples`
6. `latexmk -pdf main.tex` in each handbook directory (`python-best-practices`, `git-github-best-practices`, `software-testing-handbook`).

Running these steps locally before pushing keeps pulls green and documentation in sync with the templates.

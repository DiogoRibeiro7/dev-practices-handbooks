# Contributing Guide

Thanks for improving the Dev Practices Handbooks. This guide keeps contributions consistent and buildable.

## Build Locally

1. Review the full build checklist in `docs/BUILD.md`.
2. Install prerequisites:
   - LaTeX (`latexmk`, TeX Live or MiKTeX)
   - Node.js 18+
   - Python 3.11+
   - Rust toolchain for `mdbook` and `lychee`
3. Platform-specific setup examples:

   Windows (PowerShell with winget):

   ```powershell
   winget install --id MiKTeX.MiKTeX -e
   winget install --id OpenJS.NodeJS.LTS -e
   winget install --id Python.Python.3.11 -e
   winget install --id Rustlang.Rustup -e
   ```

   macOS (Homebrew):

   ```bash
   brew install node python@3.11 rust
   brew install --cask mactex-no-gui
   ```

   Ubuntu/Debian:

   ```bash
   sudo apt-get update
   sudo apt-get install -y latexmk texlive-latex-extra texlive-fonts-recommended texlive-lang-english nodejs npm python3 python3-pip cargo
   ```
4. Install repo tools:

   ```bash
   npm install
   python -m pip install --user ruff black
   cargo install mdbook lychee
   ```

5. Run the main checks:

   ```bash
   mdbook build docs/book
   npm run lint
   lychee --config lychee.toml
   ruff check software-testing-handbook/examples
   black --check software-testing-handbook/examples
   ```

6. Build any handbook you changed:

   ```bash
   cd python-best-practices && latexmk -pdf main.tex
   cd git-github-best-practices && latexmk -pdf main.tex
   cd software-testing-handbook && latexmk -pdf main.tex
   ```

## Add A Chapter

1. Start with `docs/CHAPTER_TEMPLATE.md` and follow the guidance in `docs/STYLE_GUIDE.md`.
2. Create the chapter file under the relevant handbook directory:
   - `python-best-practices/chapters/`
   - `git-github-best-practices/latex/`
   - `software-testing-handbook/chapters/`
3. Update the handbook table of contents in the `main.tex` file to include the new chapter.
4. Update the roadmap in `docs/expansion-plan.md` and the site index at `docs/book/src/new-chapters.md` if you introduce new topics.
5. Add deterministic examples under `*/examples/chXX/` (when applicable) and include a `README.md` with run steps.

## Style Rules

- Use stable event names in logging/telemetry examples.
- Keep field naming consistent across examples and narrative (snake_case vs camelCase, etc.).
- Ensure all code examples run as written; provide commands and pin versions when needed.

## Examples & Automation

- Lint markdown with `npm run lint`.
- Spell-check markdown with `npm run lint:spell` (uses `.cspell.json`).
- Keep Python example code formatted with `black --check <path>` and linted by `ruff check <path>`.
- Link-check markdown with `lychee --config lychee.toml`.
- Build the documentation site with `mdbook build docs/book`.
- Build LaTeX handbooks using `latexmk -pdf main.tex` inside each handbook directory.

## Pull Requests

- Run all checks locally before pushing (see `docs/BUILD.md`).
- Include generated PDF artifacts or describe how to produce them.
- Ensure any new `*.md` files pass `markdownlint` and `cspell`.

## Testing & CI

CI executes the above checks on every pull request. Fix any detected issues locally before merging to keep the main branch green.

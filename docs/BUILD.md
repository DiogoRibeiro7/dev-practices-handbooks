# Build & Verification

## Toolchain Overview

- **LaTeX** (`latexmk -pdf main.tex`) builds each handbook from `python-best-practices`, `git-github-best-practices`, and `software-testing-handbook`.
- **Documentation site** is rendered with [`mdBook`](https://book.melloware.com/mdbook/) from `docs/book`. It surfaces the template/style guides, new chapter roadmap, and build/check instructions.
- **Automation checks** cover Markdown formatting, spelling, link health, and Python example linting.

## Prerequisites

1. Install LaTeX: `sudo apt-get install -y latexmk texlive-latex-extra texlive-fonts-recommended texlive-lang-english` (Linux) or use TeX Live/MiKTeX on Windows.
2. Install the Rust toolchain, which is needed for `mdbook` and `lychee` (`curl https://sh.rustup.rs -sSf | sh`).
3. Install `mdbook` and `lychee` via Cargo:

   ```bash
   cargo install mdbook
   cargo install lychee
   ```

4. Install Node.js (>=18) and Python 3.11+.
5. Install toolchain dependencies:

   ```bash
   npm install
   python -m pip install --user ruff black
   ```

## Local Build Steps

1. **Run documentation build:** `mdbook build docs/book`
2. **Run Markdown lints:** `npm run lint`
3. **Run link check:** `lychee --config lychee.toml`
4. **Check Python examples:**

   ```bash
   ruff check software-testing-handbook/examples
   black --check software-testing-handbook/examples
   ```

5. **Build LaTeX handbooks:**

   ```bash
   cd python-best-practices && latexmk -pdf main.tex
   cd git-github-best-practices && latexmk -pdf main.tex
   cd software-testing-handbook && latexmk -pdf main.tex
   ```

## Continuous Integration

CI runs the same checks:

- Markdown linting + spelling (`npm run lint`).
- Link verification (`lychee --config lychee.toml`).
- Python formatting/lint (`ruff` and `black`).
- `mdbook build docs/book`.
- `latexmk` builds for each handbook. PDF artifacts are uploaded for review.

# Logging Book

A concise LaTeX book about writing good log messages and structured logging.

## Build

You can build with either `make` or the script. Both use `pdflatex` and place output in `dist/`.

```bash
make pdf
```

```bash
./scripts/build.sh
```

To clean LaTeX artifacts:

```bash
make clean
```

## Repo Files

- `LICENSE`
- `CONTRIBUTING.md`
- `CHANGELOG.md`
- `CODEOWNERS`

## Structure

```
logging-book/
  main.tex
  preamble.tex
  chapters/
    01-intro.tex
    02-levels.tex
    03-messages.tex
    04-structured.tex
    05-errors.tex
    06-safety.tex
    07-lambda.tex
    08-catalogue.tex
    09-testing.tex
  appendices/
    A-quickref.tex
  assets/
  scripts/
    build.sh
    clean.sh
    check.sh
  dist/
  Makefile
  .gitignore
  LICENSE
  CONTRIBUTING.md
  CHANGELOG.md
  CODEOWNERS
```

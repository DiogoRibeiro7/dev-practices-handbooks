# QA Report: Database Design Book
Date: 2026-02-02

## Quality Gates
- Book compiles with latexmk cleanly: PASS (latexmk -C, then `latexmk -pdf -interaction=nonstopmode -halt-on-error book/main.tex`)
- No missing references, no undefined control sequences: PASS (no matches in `build/main.log`)
- Every chapter has learningobjectives + chaptersummary + exercisebox: FAIL
  - Missing in `database_design/chapters/ch03-keys-identity.tex`
  - Missing in `database_design/chapters/ch17-conclusion-next-steps.tex` (file exists but is not included in `database_design/book/main.tex`)
- Notation is consistent and uses \rel and \term macros: FAIL (not fully audited; macros are used, but no completeness check)
- Case study chapters compile and include their diagrams: PASS (`clinic-case.pdf` and `library-case.pdf` included)
- CI builds and uploads artifacts: FAIL (workflow present, not executed in this QA run)

## TODOs / Known Issues
- Add learning objectives, chapter summary, and exercise box to `database_design/chapters/ch03-keys-identity.tex`.
- Decide whether to remove or populate `database_design/chapters/ch17-conclusion-next-steps.tex`, or include it in the build.
- Audit notation for any relation names or terms not using \rel/\term and normalize.
- Run the GitHub Actions workflow to validate PDF artifact upload.
- Optional: resolve overfull/underfull hbox warnings in `build/main.log` (formatting only).

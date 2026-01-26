# Build Instructions

This book uses LaTeX with `latexmk` for a one-command PDF build.

## Build command

```bash
cd product-management-handbook
latexmk -pdf main.tex
```

## Clean build artifacts

```bash
cd product-management-handbook
latexmk -C
```

## Notes
- The build does not require images. If you add images later, prefer paths under `product-management-handbook/images/`.
- The appendix is included as a LaTeX file generated from Markdown-compatible content.

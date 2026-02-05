# Contributing

Thanks for contributing to Logging Book.

## Build Locally

```bash
make pdf
```

Or use the script:

```bash
./scripts/build.sh
```

## Add a Chapter

1. Create a new file under `chapters/` using the next sequence number.
2. Add 2–4 sections and at least one checklist, table, or code listing.
3. Include it in `main.tex` with `\include{chapters/NN-name}`.
4. Run `make pdf` before opening a PR.

## Style Rules

- Use stable event names. Prefer `noun.verb` or `domain.action`.
- Keep field names consistent across chapters. Use `event`, `level`, `outcome`, `ids`, `fields`.
- Code examples must run. Prefer `python` with stdlib `logging` and `extra={...}`.
- Avoid fluff. Focus on practical log message guidance.

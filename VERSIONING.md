# Versioning

This project uses lightweight draft tags to track beta milestone artifacts. Follow these conventions:

- **Draft tags**: use `draft-vX.Y` (e.g., `draft-v0.1`, `draft-v0.2`). Tag each beta iteration after merging the latest content updates and rebuilding the PDFs. Increase `X` for major structural changes and `Y` for smaller rounds.
- **Release tags**: reserve `vX.Y.Z` for fully reviewed releases once the manuscript stabilizes and all chapters are complete.
- **Changelog**: record major updates (new chapters, examples, tooling) in `README` or a dedicated `CHANGELOG.md` entry when moving from one draft tag to another.

Before tagging `draft-vX.Y`:
1. Run the `npm run lint` checks plus `latexmk -pdf main.tex` for each handbook.
2. Update `dist/` with the new PDFs and mention the tag in `docs/BETA_READER.md` if needed.
3. Create the annotated tag: `git tag draft-vX.Y` and push it with `git push origin draft-vX.Y`.

# Style Guide

## Voice and Tone

- Use active voice and address the reader as a teammate (e.g., "You can model risk.").
- Keep sentences short to medium length; break longer ideas into two sentences.
- Avoid flowery language—be practical and direct.

## Formatting

- Always start chapters with `\\section{Motivation and Learning Objectives}` followed by the template order.
- Use `\\subsection{}` for each core concept, anti-pattern, etc., instead of informal headings.
- Insert `TODO:` markers in bold when you need to return to a section (e.g., `\\textbf{TODO: add regression checklist.}`).
- Inline code should be wrapped in `\\texttt{}`; longer examples must use `\\begin{lstlisting}[language=...]`.
- Use `itemize` for unordered lists, `enumerate` for exercises or ordered steps.

## Terminology

- Prefer "suite" over "test" when describing a collection of checks.
- Use "risk profile," "gate," "automation pipeline," "observability," and "charter" consistently.
- Spell out acronyms the first time with parentheses (e.g., "Service Level Indicator (SLI)").

## Code Samples

- Always specify the language in the `lstlisting` environment (e.g., `language=python`, `language=yaml`).
- Keep samples short (~10 lines) and focused on illustrating one idea.
- When referencing CI, prefer YAML that can be executed in GitHub Actions, GitLab CI, or Azure Pipelines.
- Add comments prefixed with `# TODO` for incomplete logic.

## Callouts and Admonitions

- Use bolded sentences to call attention (e.g., `\\textbf{Note:} ...`).
- Keep callouts brief and place them near the relevant content.
- Do not rely on LaTeX packages beyond what is already included in the main document.

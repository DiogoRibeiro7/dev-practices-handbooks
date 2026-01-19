# ISSUE LIST

## High Priority
1. Chapters `chapter07_doubles.tex` through `chapter13_refactoring.tex` are still scaffolds of TODOs with no narrative, worked example, or exercises finished; they must be fully authored before the book can claim coverage for those advanced topics (`software-testing-handbook/chapters/chapter0*.tex`).
2. `chapter10_flaky.tex` leaves its anti-pattern list, worked example, checklist, and exercises empty—add concrete detection/quarantine guidance and runnable examples to fulfill the goal of that chapter.
3. Many referenced example scripts (`software-testing-handbook/examples/ch02/pyramid_planner.py`, `examples/ch03/ci_pipeline.py`, etc.) have not been executed as part of this review; add verification steps in their READMEs and/or CI jobs so the manuscript can claim the code works as described.

## Medium Priority
1. Tighten the transition between the strategy chapters and automation (Chapter 2 → 3); consider a short “what follows” note in Chapter 2 before the gating discussion if the shift feels abrupt.
2. The new “test doubles taxonomy” section should be referenced wherever mocks/stubs appear so terminology stays consistent across all chapters (expanding Chapter 7 and later sections is the natural place).

## Low Priority
1. Double-check there are no residual repeated headings or duplicated “Further Reading” sections elsewhere; Chapter 2 had a duplication that has been removed, but run a quick grep for `Further Reading` sections to ensure each chapter ends cleanly.

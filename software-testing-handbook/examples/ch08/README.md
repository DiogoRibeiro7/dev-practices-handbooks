# Chapter 8 Property Testing Examples

This example demonstrates Hypothesis-driven invariants, seed control, and shrink-friendly property checks so CI can rerun counterexamples deterministically.

## Requirements

- Python 3.11+
- Install dependencies: `pip install hypothesis`

## Run locally

```bash
cd software-testing-handbook
python examples/ch08/sort_invariants.py
```

## Run tests

```bash
cd software-testing-handbook
pytest examples/ch08
```

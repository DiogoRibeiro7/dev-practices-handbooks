# Chapter 2 Examples

These scripts model the testing pyramid and release gating logic described in Chapter 2. They are intentionally small and deterministic.

## Requirements

- Python 3.11+ (or newer)
- Install `pytest` for running the test harness: `pip install pytest`

## Run locally

```bash
cd software-testing-handbook
python examples/ch02/pyramid_planner.py
```

## Run tests (CI-friendly)

```bash
cd software-testing-handbook
pytest examples/ch02
```

These commands require no additional dependencies beyond standard library and pytest.

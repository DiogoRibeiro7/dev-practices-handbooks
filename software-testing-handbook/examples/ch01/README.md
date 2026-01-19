# Chapter 1 Examples

These scripts demonstrate foundations from Chapter 1. Run them locally with Python 3.11+.

## Prerequisites

- Python 3.x
- (Optional) virtual environment for isolation.

## Running locally

```bash
cd software-testing-handbook
python examples/ch01/risk_score.py
python examples/ch01/deterministic_assertions.py
```

## Running in CI

Add a job step (GitHub Actions example):

```yaml
- name: Run chapter 1 examples
  run: python examples/ch01/risk_score.py && python examples/ch01/deterministic_assertions.py
```

No additional packages are required.

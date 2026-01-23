# Chapter 9 Contracts & Integration Examples

This example shows how contracts and seeded provider data keep integration suites deterministic even when containers spin up new environments.

## Requirements

- Python 3.11+
- Install dependencies: none beyond the standard library.

## Run locally

```bash
cd software-testing-handbook
python examples/ch09/contract_runner.py
```

Run the generator after your Docker Compose stack seeds data so the manifest points to the same `offers.json` fixture that your provider exposes.  

## Run tests

```bash
cd software-testing-handbook
pytest examples/ch09
```

# Chapter 6 Capstone Examples

The capstone readiness script aggregates the earlier chapters into one release decision tool.

## Requirements

- Python 3.11+
- Install `pytest` for the harness.

## Generate a CI manifest

```bash
cd software-testing-handbook
python examples/ch06/generate_ci_manifest.py
```

The command writes `examples/ch06/ci-artifacts/readiness_manifest.json`, emulating the JSON payload a CI job would publish before the capstone runs.

## Run locally

```bash
cd software-testing-handbook
python examples/ch06/capstone_readiness.py --manifest examples/ch06/ci-artifacts/readiness_manifest.json
```

The script prints pillar verdicts, links each status to the artifact manifest, and emits remediation steps when readiness is not `ready`.

## Run tests

```bash
cd software-testing-handbook
pytest examples/ch06
```

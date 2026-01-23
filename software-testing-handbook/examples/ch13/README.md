# Chapter 13 Refactoring Examples

This module contains the refactored `ReportHandler` plus helpers that let you inject databases, notifiers, and clocks for deterministic tests.

## Run the worked example

```bash
python - <<'PY'
from examples.ch13.report_handler import build_default_handler

handler = build_default_handler()
handler.run({"id": "report-1", "owner": "ops"})
PY
```

The example prints the notification to stdout so you can verify the production wiring independently of tests.

## Run the tests

```bash
pytest software-testing-handbook/examples/ch13
```

The test suite uses fakes and a frozen clock to keep the handler deterministic and fast in CI.

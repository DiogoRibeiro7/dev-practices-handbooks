# Chapter 13 Refactoring Examples

This example shows a before/after handler refactor. The "before" version constructs its dependencies internally, making deterministic tests hard. The "after" version uses constructor injection and exposes a production builder.

## Before: hard-wired dependencies

```bash
python - <<'PY'
from examples.ch13.report_handler_before import ReportHandlerBefore

handler = ReportHandlerBefore()
print(handler.run({"id": "order-42", "owner": "ops"}))
PY
```

The handler builds the database client, notifier, and clock inside the call. That makes it difficult to fix time or capture notifications in tests without patching.

## After: constructor injection + fakes

```bash
python - <<'PY'
from datetime import datetime, timezone

from examples.ch13.fakes import FakeClock, FakeDB, SpyNotifier
from examples.ch13.report_handler import ReportHandler

db = FakeDB(records={"order-42": {"summary": "ok"}})
clock = FakeClock(datetime(2026, 1, 1, 12, 0, tzinfo=timezone.utc))
notifier = SpyNotifier()

handler = ReportHandler(db=db, notifier=notifier, clock=clock)
print(handler.run({"id": "order-42", "owner": "ops"}))
print(notifier.sent_payloads)
PY
```

## Production wiring

```bash
python - <<'PY'
from examples.ch13.report_handler import build_default_handler

handler = build_default_handler()
print(handler.run({"id": "order-42", "owner": "ops"}))
PY
```

## Run the tests

```bash
pytest software-testing-handbook/examples/ch13
```

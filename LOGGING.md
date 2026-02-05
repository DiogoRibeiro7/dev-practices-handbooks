# Logging Style Guide

This is a short, practical guide for writing log messages that are easy to read, search, and act on.

## Message Rules
- Log one event per line.
- Use stable wording and stable event names.
- Write in present tense.
- Put the event name and outcome in the message.
- Keep messages readable without fields.

## Level Rules
Use these levels consistently. Treat them as filters.

| Level | Use it when |
| --- | --- |
| DEBUG | Internal state for diagnosis and safe to disable. |
| INFO | Expected event with a normal outcome. |
| WARNING | Unusual outcome but the operation completes. |
| ERROR | The event fails or data cannot be processed. |
| CRITICAL | The process cannot continue safely. |

## Required Fields
Every structured log should include these fields. Use the same names across services.
- `event`
- `request_id` or `correlation_id`
- `duration_ms`
- `status`

## Error Handling Rule
- Always include stack traces for exceptions.

## Safety Rules
- Do not log secrets, tokens, or passwords.
- Avoid PII. Prefer opaque IDs instead of names or emails.
- If you must log sensitive data, redact before logging.

## Examples (Python)

```python
import logging

logger = logging.getLogger("service")

logger.info(
    "job.start: status=ok",
    extra={
        "event": "job.start",
        "request_id": "req_9a7b",
        "duration_ms": 3,
        "status": "ok",
        "fields": {"attempt": 1},
    },
)

logger.info(
    "http.call: status=ok",
    extra={
        "event": "http.call",
        "correlation_id": "corr_41c2",
        "duration_ms": 120,
        "status": "ok",
        "fields": {"url_host": "api.example.com", "status_code": 200},
    },
)

logger.warning(
    "http.retry: status=retry",
    extra={
        "event": "http.retry",
        "correlation_id": "corr_41c2",
        "duration_ms": 0,
        "status": "retry",
        "fields": {"attempt": 2, "backoff_ms": 1000, "status_code": 503},
    },
)

logger.info(
    "queue.processed: status=ok",
    extra={
        "event": "queue.processed",
        "request_id": "req_55d1",
        "duration_ms": 22,
        "status": "ok",
        "fields": {"message_id": "m-9", "rows_out": 3},
    },
)

try:
    raise ValueError("schema mismatch")
except Exception:
    logger.exception(
        "record.validate: status=error",
        extra={
            "event": "record.validate",
            "request_id": "req_aa12",
            "duration_ms": 5,
            "status": "error",
            "fields": {"rows_in": 1, "rows_out": 0},
        },
    )

logger.error(
    "storage.write: status=error",
    extra={
        "event": "storage.write",
        "correlation_id": "corr_7c99",
        "duration_ms": 640,
        "status": "error",
        "fields": {"table": "events", "attempt": 3},
    },
)

logger.info(
    "batch.completed: status=ok",
    extra={
        "event": "batch.completed",
        "request_id": "req_1f02",
        "duration_ms": 4100,
        "status": "ok",
        "fields": {"rows_in": 12000, "rows_out": 11985},
    },
)

logger.warning(
    "validation.summary: status=warn",
    extra={
        "event": "validation.summary",
        "correlation_id": "corr_2b31",
        "duration_ms": 88,
        "status": "warn",
        "fields": {"rule_set": "core", "rows_in": 1000, "rows_out": 992},
    },
)
```

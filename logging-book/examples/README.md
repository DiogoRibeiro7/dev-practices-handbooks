# Logging Examples

All scripts are stdlib-only and runnable directly with Python 3.11+.

## Run Scripts

```bash
python examples/stdlib_logging_setup.py
python examples/json_formatter.py
python examples/lambda_contextvars.py
```

## What Each Script Does

- `stdlib_logging_setup.py`: Basic logging configuration with extra fields added via `extra`.
- `json_formatter.py`: Custom JSON formatter that emits one JSON object per log line.
- `lambda_contextvars.py`: ContextVar + logging filter example that simulates request-scoped context.

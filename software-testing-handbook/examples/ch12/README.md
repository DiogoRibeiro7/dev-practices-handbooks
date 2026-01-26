# Chapter 12 CI Strategy Examples

The selective runner demonstrates how CI decides which suites to run, how cache keys are generated, and how to record why each job was selected.

## Run the planner

```bash
python - <<'PY'
from examples.ch12.selective_runner import plan_pipeline

payload = {
    "changed_files": ["src/api/service.py", "tests/unit/test_api.py"],
    "durations": {"lint": 45, "unit": 180, "smoke": 90},
    "lock_hash": "v1-deps",
    "repo_hash": "main-abc123",
}

for job in plan_pipeline(payload):
    print(f"{job.name}: runner={job.runner} cache={job.cache_key} needs={job.needs}")
PY
```

The planner writes `ci/changed_files.json` with the triggering files, selected jobs, and critical path time so reviewers can see why the pipeline was chosen.

## Run the tests

```bash
pytest software-testing-handbook/examples/ch12
```

The test suite verifies job selection, cache key determinism, critical path computation, and the selection report artifact.

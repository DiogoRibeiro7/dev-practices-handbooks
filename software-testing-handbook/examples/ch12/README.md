# Chapter 12 CI Strategy Examples

The selective runner demonstrates how CI decides which suites to run and how cache keys are generated for each job.

## Run the planner

```bash
python - <<'PY'
from examples.ch12.selective_runner import plan_pipeline

payload = {
    "changed_files": ["services/api/main.py", "contracts/payments.yml"],
    "lock_hash": "v1-deps",
}

for job in plan_pipeline(payload):
    print(job)
PY
```

The output lists the jobs, their runners, cache keys, and dependencies so CI can render the matrix before execution.

## Run the tests

```bash
pytest software-testing-handbook/examples/ch12
```

The test suite verifies that the planner picks the right jobs and that cache keys remain stable across invocations.

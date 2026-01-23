# Flaky Detector (Chapter 10)

The rerun harness in this chapter captures every attempt, seeds the RNG deterministically, and emits a verdict once a passing run occurs.

## Running locally

```bash
python examples/ch10/flaky_detector.py --seed 42 --retries 5
```

The script prints the rerun verdict and a summary of every attempt so you can correlate failures with seeds or timing signals.

## Running the tests

```bash
pytest examples/ch10
```

The tests verify the harness stops on success, reports failure after exhaustion, and exposes a human-readable summary string suitable for CI logs.

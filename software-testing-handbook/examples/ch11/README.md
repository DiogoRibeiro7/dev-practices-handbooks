# Chapter 11 coverage lab

This mini-project shows how line and branch coverage diverge when a risk-critical path is exercised but another branch remains uncovered.

## Files

- `order_processor.py`: calculates shipping cost with thresholds, coupons, and validation branches.
- `test_order_processor.py`: covers the happy path, coupon discount, free shipping, and error states.
- (Intentionally uncovered) the `FREESHIP` coupon branch and other discounts that drop shipping to zero remain untested so the coverage report highlights the gap.

## How to run

```bash
pytest --cov=examples.ch11 --cov-branch --cov-report=term --cov-report=html
```

After the run, open `htmlcov/index.html` in a browser to see the detailed coverage map and the uncovered `FREESHIP` branch.

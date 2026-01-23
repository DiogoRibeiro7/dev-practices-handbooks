# Chapter 11 Coverage Examples

This folder contains the minimal `Order` domain logic used to demonstrate line/branch coverage trade-offs.

## Run the worked example

```bash
python - <<'PY'
from examples.ch11.order_processor import Order, shipping_cost, apply_discount

orders = [
    Order(total=150.0, has_coupon=False, items=["widget"]),
    Order(total=18.0, has_coupon=True, items=["gadget"]),
]

for order in orders:
    print(f"order={order.total} coupon={order.has_coupon} shipping={shipping_cost(order)}")
    print(f"discounted total={apply_discount(order, 5.0)}")
PY
```

The snippet above exercises the same functions covered by the tests and highlights which branches remain unverified when you add new business rules.

## Run the tests with coverage

```bash
pytest --cov=examples.ch11 --cov-report=term --cov-report=html
```

Expose the HTML report from `htmlcov/index.html` or include the terminal summary in pull requests. CI should gate the merge on branch coverage thresholds (e.g., `--cov-branch --cov-fail-under=80`) so the coverage numbers drive risk-based investments.

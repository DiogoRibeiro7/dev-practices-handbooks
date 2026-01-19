# Proposed New Chapters for Software Testing Handbook

<!-- markdownlint-disable MD022 MD032 -->

These chapters build on the existing six and dig into advanced practices that engineers encounter after the fundamentals are in place. Each chapter keeps the practical, testable tone established earlier and points the reader toward deterministic, CI-friendly examples.

## Chapter 07 – Designing Test Doubles and Mocking Discipline
### Overview
Distinguish between stubs, mocks, fakes, and spies while grounding decisions in feedback loops and system complexity. Emphasize when production code should own behavior versus when controlled doubles unlock fast, reliable feedback.

### Learning objectives
- Enumerate the types of test doubles and the design decisions that drive each choice.
- Recognize when mocking interaction versus asserting state is appropriate.
- Understand the risks of overmocking and how to protect teams from brittle specs.

### Section outline
1. Motivation: when real dependencies slow down feedback.
2. Taxonomy of doubles (stubs, mocks, spies, fakes).
3. Mocking vs state verification.
4. Anti-patterns: mocking concrete implementations, excessive interaction assertions.
5. Tactics for incremental replacement: seeding doubles in CI, using golden fixtures.
6. Worked example placeholder (TODO: show a repository/service extracting a disk or network dependency into an interface and injecting a fake).

### Example plan
- Example: `examples/ch07/doubles.py` + `README.md`.
- Small service that calls an external rating API; double the API client with a fake that can assert calls without hitting the network.
- Provide instructions to run `python -m pytest` with the fake configured via dependency injection.
- Ensure the test harness seeds deterministic responses and documents how to swap to a slower integration test.

### Exercises plan
1. Identify three functions in your repo where a double could replace a slow dependency and sketch the interface.
2. Write a test that uses a spy to assert a retry loop executes the expected number of times.
3. Refactor a test that currently mocks a concrete class into one that depends on an interface.
4. Create a checklist for when a double should be added vs when a full dependency should stay in the test.

## Chapter 08 – Property-Based and Meta Testing
### Overview
Demonstrate how to express invariant-driven expectations that scale beyond example-based assertions, including how to tame randomness and keep suites deterministic.

### Learning objectives
- Describe the difference between example-based and property-based testing.
- Use property-based tools (Hypothesis, QuickCheck) to cover wide input spaces.
- Keep property-based suites deterministic by freezing seeds and shrinking runs.

### Section outline
1. Why rely on invariants: catching edge cases without enumerating them.
2. Property-based toolkit overview for mainstream languages.
3. Designing properties: inputs, state, and expected invariants.
4. Handling randomness/IO: seeding RNG, mocking clocks, isolating side effects.
5. Anti-pattern: property tests that mirror the implementation under test.
6. Worked example: placeholder for sorting/invoice invariants with Hypothesis.

### Example plan
- Repository: `examples/ch08/property_tests/`.
- Example script that verifies a payment batching algorithm preserves totals and is independent of event order.
- Run with `pytest --maxfail=1` and document how to inspect failures and shrink inputs.

### Exercises plan
1. Convert an existing example-based test into a property test with at least two generated parameters.
2. Add a custom strategy to generate realistic domain objects.
3. Run a property test with a fixed seed and show that re-running produces the same failure.
4. Document an anti-pattern you see in your codebase where properties add little value.

## Chapter 09 – Consumer-Driven Contracts and Integration Testing with Containers
### Overview
Cover the bridge between fast unit-like tests and slow-but-real integration suites by codifying service contracts and running containers in CI.

### Learning objectives
- Explain what consumer-driven contracts are and when to prefer them.
- Compose integration tests that spin up containerized dependencies.
- Define a lightweight contract verification workflow for multi-team APIs.

### Section outline
1. Why contracts: reducing back-and-forth between teams.
2. Tooling overview (e.g., Pact, Spring Cloud Contract, Hoverfly).
3. Writing consumer contracts and publishing them to a broker.
4. Containerized integration tests: using docker-compose/testcontainers for databases or queues.
5. Anti-pattern: duplicating production logic in contract descriptions.
6. Worked example: placeholder contract test vs containerized integration harness.

### Example plan
- Example folder: `examples/ch09/contracts-integration/`.
- Provide a simple API client stub that validates a contract published by the consumer.
- Include a docker-compose file that brings up a fake downstream service for integration tests.
- Document how to run the contract verification in CI (e.g., `docker compose run tests`).

### Exercises plan
1. Write a consumer-driven contract for an HTTP endpoint your team depends on.
2. Create a test that hits a Docker container (database or message queue) with deterministic data.
3. Identify the minimum set of contract assertions that would detect breaking changes.
4. Draft a workflow for publishing contract changes safely.

## Chapter 10 – Flaky Tests: Detection, Quarantine, and Fixes
### Overview
Teach patterns for surfacing nondeterministic tests, building quarantines, and investing in fixes instead of simply ignoring flakes.

### Learning objectives
- Detect flaky tests through rerun policies and observability (logs/traces).
- Apply quarantine strategies without degrading confidence.
- Diagnose common sources of flakiness (timing, shared state, resource contention).

### Section outline
1. Types of flakes (order, environment, timing).
2. Detection tactics: reruns, historical failure rates, dashboards.
3. Quarantine workflow: tagging, monitoring, triage guidelines.
4. Fix strategies: better isolation, deterministic seeds, service virtualization.
5. Anti-pattern: using reruns as permanent band-aid.
6. Worked example: placeholder script that reruns flaky test with recorded logs.

### Example plan
- Example path: `examples/ch10/flaky_detection/`.
- Provide a deterministic harness that intentionally fails intermittently (e.g., time-based) and shows how to reproduce/fix it.
- Include a helper to rerun tests up to N times, log outcomes, and emit JSON summary for CI.

### Exercises plan
1. Analyze recent CI history; flag tests that fail more than once in a day.
2. Implement a quarantine label/tag in your test suite.
3. Convert an order-dependent test to one that resets home state before each run.
4. Document a hallucinated cause of flakiness and propose mitigation steps.

## Chapter 11 – Coverage, Risk, and Confidence
### Overview
Clarify coverage metrics, their limits, and how to use them alongside risk-based lists to guide testing effort.

### Learning objectives
- Define line, branch, and mutation coverage and what they prove (and do not).
- Relate coverage goals to business risk and release confidence.
- Integrate coverage tooling into CI without making it a checkbox.

### Section outline
1. What coverage is (lines hit, branches exercised) and what it is not (proof of correctness).
2. Interpreting gap reports; pairing coverage with smoke/regression tests.
3. Using lightweight mutation tests for high-risk paths.
4. Anti-pattern: gating builds solely on hitting 100% coverage.
5. Worked example: placeholder measuring coverage for a small service and correlating with targeted tests.

### Example plan
- Example folder: `examples/ch11/coverage/`.
- Script that runs `pytest --cov` to capture data, outputs summary, and highlights low coverage areas.
- Document how to target tests to raise coverage for specific modules, not arbitrarily chasing 100%.

### Exercises plan
1. Run coverage locally and identify the three least-coveted modules.
2. Pair coverage output with a list of high-risk requirements.
3. Sketch a mutation test for a guard clause in your codebase.
4. Write a short note explaining why 100% coverage is not a sufficient goal.

## Chapter 12 – CI Strategy for Testing
### Overview
Explain how to organize CI pipelines so tests run fast, in parallel, and deliver useful reports back to engineers.

### Learning objectives
- Configure test parallelism, caching, and splitting strategies in CI systems.
- Implement test selection (changed-files, smoke-first) for large suites.
- Provide actionable reporting and alerts to developers.

### Section outline
1. Philosophy of CI vision: fast, reliable, informative.
2. Parallelism: chunking suites, matrix jobs, and container reuse.
3. Caching test dependencies/artifacts.
4. Selective testing: git diff-based filters, tags, and prioritizing critical tests.
5. Reporting: flake dashboards, annotated diffs, and artifacts.
6. Worked example: placeholder GitHub Actions or GitLab CI job definition.

### Example plan
- Example directory: `examples/ch12/ci-strategy/` with sample GitHub Action workflow.
- Workflow that runs a fast smoke matrix and a slower regression job, both caching virtualenvs.
- Provide CLI commands to rerun a failed stage locally for debugging.

### Exercises plan
1. Sketch a CI job graph for your repo and identify long-running components.
2. Implement a simple file-change filter for selecting smoke tests.
3. Configure artifact upload for failed tests and view them manually.
4. Document how CI could notify teams about flaky or skipped jobs.

## Chapter 13 – Refactoring for Testability
### Overview
Teach developers to evolve code into shapes that are easy to test, reducing reliance on fragile hooks.

### Learning objectives
- Identify seams, dependency injection points, and how to avoid global state.
- Refactor legacy modules with high coupling into testable units.
- Balance testability with pragmatic delivery timelines.

### Section outline
1. Seams and the economic case for refactoring.
2. Dependency inversion and factories for external resources.
3. Breaking large classes into smaller collaborators.
4. Anti-pattern: tests that only pass because they know private internals.
5. Worked example: placeholder refactor of a monolithic handler into testable components.

### Example plan
- Example path: `examples/ch13/refactoring/` with before/after service files.
- Show how to break a handler that directly instantiates clients into one that accepts interfaces.
- Provide tests for both the old and new implementation to highlight improvements.

### Exercises plan
1. Pick a class that is hard to test and outline the seams you would need.
2. Refactor a function to accept collaborators via parameters or constructors.
3. Write a regression test that fails before refactoring and passes afterward.
4. Document a trade-off you made between testability and simplicity.

These chapters can be published sequentially after the foundational six while keeping the new `examples/ch0X/` structure for runnable code.
\n<!-- markdownlint-enable MD022 MD032 -->

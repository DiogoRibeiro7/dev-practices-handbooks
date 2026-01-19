# New Chapters Roadmap

Following the foundational six chapters, the new material deepens practical testing craft. Each entry below summarizes the promise, the demonstration strategy, and the exercises planned. The full write-up lives in `docs/NEW_CHAPTERS.md`.

| Chapter | Focus | Example plan | Exercises |
| --- | --- | --- | --- |
| 7 – Test Doubles & Mocking | Choosing stubs, mocks, and fakes with discipline | Fake the rating API client under test; keep responses deterministic | Design doubles for slow dependencies, refactor an over-mocked suite |
| 8 – Property-Based Testing | Express invariants to cover wide input spaces | Hypothesis/QuickCheck build that verifies batching invariants with seeded randomness | Rewrite an example-based test, add custom strategies, freeze seeds |
| 9 – Contracts & Containerized Integration | Consumer-driven contracts + docker-based integration checks | Pact or similar contract test with a containerized stub service | Author a contract, run docker-compose test, define minimum asserts |
|10 – Flaky Tests | Detect, quarantine, and fix nondeterministic suites | Harness that reruns a time-sensitive test with logging for CI | Analyze CI history, quarantine a test, document fixes |
|11 – Coverage, Risk & Confidence | Interpreting coverage metrics and pairing them with risk assessments | Targeted coverage run with pytest-cov + mutation glimpses | Identify risk-clustered gaps, sketch mutation tests, explain 100% myths |
|12 – CI Strategy for Test Suites | Parallelization, caching, selection, reporting | GitHub Actions workflow that shards smoke/regression jobs with caching | Diagram job graph, add change-aware filters, upload artifacts |
|13 – Refactoring for Testability | Reducing coupling to make tests easier to write | Refactor a handler that currently instantiates dependencies into injectable pieces | Pick a hard-to-test class, refactor with seams, write regression tests |

Each chapter will live under `software-testing-handbook/chapters/chapter0X_*.tex` with code under `software-testing-handbook/examples/ch0X/`. Keep TODO markers where content is pending and follow the template defined in `docs/CHAPTER_TEMPLATE.md`.

---
name: python-tests
description: Use when writing or reviewing Python tests: "write tests for this", "review the tests", "is this covered?".
---

# Python Tests

Every behavior change ships with tests. The questions are which tests, and whether they test the right thing.

## What to test

Test **your logic**, not Python and not third-party libraries:

| Test this | Don't test this |
| --- | --- |
| Custom `__eq__`, `__hash__` with business rules | `frozen=True`, `kw_only=True`, `eq=True`: dataclass guarantees |
| `default_factory` with domain logic | `issubclass(X, Y)`: a static fact, visible from the code |
| String formatting in custom exceptions | `str(Exception("msg")) == "msg"`: a Python guarantee |
| Decorator behavior | `isinstance(x, T)` when the type is obvious from the constructor |
| State transitions, validation, business invariants | That a third-party client can connect or call its own API |
| Error wrapping (a domain error raised from a driver error) | That a mock returns what you told it to return |

Parametrize repetitive tests. Don't write N identical tests when one `@pytest.mark.parametrize` covers them all.

But not everything needs a test. A test earns its place only if it would fail when your logic breaks. One that tests nothing real is rejected: don't write it, propose deleting it in a review, and decline it when a reviewer asks for it.

## Test types

- **Unit**: mock all infrastructure, test logic in isolation.
- **Integration**: run real infrastructure (a database, a cache, object storage) in containers. Verify integration-specific behavior: data survives a round-trip, transactions roll back, a presigned URL is reachable. Don't repeat unit assertions against real infra.
- **End-to-end**: a deployed server over real HTTP with real third-party integrations. Each test provisions its own data, runs the scenario, and cleans up after itself. Required environment is configured up front; the suite hard-fails when it isn't rather than silently skipping.

Mark integration and end-to-end tests with pytest markers so they can be selected and excluded.

## Mocking

- No `monkeypatch`, no `patch`. Inject mocks through constructors.
- Exception: stdlib process-control primitives (`os.exec*`, `os.fork`, and the like). Patching is the only way to test paths that would otherwise replace the test process.
- Always use `spec`: `AsyncMock(spec=IUserRepository)`.
- Prefer `assert_awaited_*` over `assert_called_*` for async mocks.

## Structure

Arrange, act, assert:

- One action per test. Two acts means two tests.
- Separate the three blocks with blank lines, not `# Arrange` comments.
- Trivial one-liners don't need the separation.

```python
def test_pull_events_clears(self) -> None:
    agg = SampleAggregate(name="root")
    agg.add_event(SampleEvent(name="first"))

    actual = agg.pull_events()

    assert actual == [SampleEvent(name="first")]
    assert agg.pull_events() == []
```

## Assertions

Compare full objects, not individual fields:

```python
expected = User(id=user_id, name="John", email="john@example.com")
assert actual == expected
```

## Organization

- **Unit tests mirror the source tree**: one test file per source file, same nesting. `src/billing/services/invoice.py` is tested by `tests/unit/services/test_invoice.py`. Deviate only for a strong, explicit reason.
- **A source file whose tests outgrow one file** gets a test subdirectory of the same name, split by concern, with shared fixtures in a local `conftest.py`. Never split into sibling files with different names: that breaks the source-to-test mapping.
- **Hoist duplicated fixtures** to the nearest `conftest.py` that covers all their users, and no higher.
- **Integration tests** are grouped by technology, then by functionality. **End-to-end tests** by scenario or user flow.
- **Never import from `tests/`.** Shared helpers and fixtures (sample entities, mappers, ORM models) live in a `testing` package inside the source tree, or in `conftest.py`.
- **No `__init__.py` in `tests/`.** Run pytest in `importlib` import mode; init files are not needed.
- Keep test files under 300 lines, unless the project sets its own limit. Split by promoting the file to a subdirectory, as above.

## Reviewing

1. Find the test files changed on the branch (diff against the default branch).
2. For every changed source file, look for the matching test change. Behavior that was added or changed without a test is a finding, not a note. So is a test that tests nothing real: propose deleting it.
3. Check the changed tests against every section above, then against the project's own testing conventions where it has them.
4. Report each finding as file and line, severity, and what to change.

> 🐣 _Test data (names, countries, dates) can be fun. Easter eggs and jokes are welcome._

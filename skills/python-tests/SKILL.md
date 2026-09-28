---
name: python-tests
description: Use when writing or reviewing Python tests: "write pytest tests for this", "review these Python tests".
---

# Python Tests

**REQUIRED BACKGROUND:** load `mikit:better-tests` first. It holds the rules for every stack; this
skill adds Python and pytest on top.

## What to test in Python

| Test this                                       | Don't test this                                                  |
|-------------------------------------------------|------------------------------------------------------------------|
| Custom `__eq__`, `__hash__` with business rules | `frozen=True`, `kw_only=True`, `eq=True`: dataclass guarantees   |
| `default_factory` with domain logic             | `issubclass(X, Y)`: a static fact, visible from the code         |
| String formatting in custom exceptions          | `str(Exception("msg")) == "msg"`: a Python guarantee             |
| Decorator behavior                              | `isinstance(x, T)` when the type is obvious from the constructor |

Parametrize with `@pytest.mark.parametrize`. Mark integration and end-to-end tests with pytest
markers.

## Mocking

- No `monkeypatch`, no `patch`. Inject mocks through constructors.
- Exception: stdlib process-control primitives (`os.exec*`, `os.fork`, and the like). Patching is
  the only way to test paths that would otherwise replace the test process.
- Always use `spec`: `AsyncMock(spec=IUserRepository)`.
- Prefer `assert_awaited_*` over `assert_called_*` for async mocks.

## Organization

- **Unit tests mirror the source tree**: one test file per source file, same nesting.
  `src/billing/services/invoice.py` is tested by `tests/unit/services/test_invoice.py`.
- **Fixtures live in `conftest.py`**: a local one inside a test directory split by concern, hoisted
  to the nearest one that covers all their users.
- **Never import from `tests/`.** Shared helpers and fixtures (sample entities, mappers, ORM models)
  live in a `testing` package inside the source tree, or in `conftest.py`.
- **No `__init__.py` in `tests/`.** Run pytest in `importlib` import mode; init files are not
  needed.

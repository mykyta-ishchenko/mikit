---
name: better-tests
description: Use when writing or reviewing tests in any language: "write tests for this", "review the tests", "is this covered?".
---

# Better Tests

Every behavior change ships with tests. The questions are which tests, and whether they test the right thing.

These rules hold in every stack. Then load the stack skill for the code under test, if it is available: `mikit:python-tests` for Python, `mikit:typescript-tests` for TypeScript and JavaScript, `mikit:playwright-tests` for browser end-to-end tests. Where a stack skill is more specific, it wins.

## What to test

Test **your logic**, not the language, the framework, or third-party libraries:

| Test this | Don't test this |
| --- | --- |
| State transitions, validation, business invariants | A guarantee of the language, runtime, or framework |
| Every branch and error path | A static fact visible from the code: a type, a base class, a config flag |
| Error wrapping: a domain error raised from a driver error | That a third-party client can call its own API |
| Custom equality, formatting, and parsing rules | That a mock returns what you told it to return |

Parametrize repetitive tests: one table-driven test, not N copies.

But not everything needs a test. A test earns its place only if it would fail when your logic breaks. One that tests nothing real is rejected: don't write it, propose deleting it in a review, and decline it when a reviewer asks for it.

## Test types

- **Unit**: replace all infrastructure, test logic in isolation.
- **Integration**: run real infrastructure (a database, a cache, object storage) in containers. Verify integration-specific behavior: data survives a round-trip, transactions roll back, a presigned URL is reachable. Don't repeat unit assertions against real infra.
- **End-to-end**: a deployed system through its real interface, HTTP or a browser, with real third-party integrations. Each test provisions its own data, runs the scenario, and cleans up after itself. Required environment is configured up front; the suite hard-fails when it isn't rather than silently skipping.

Tag integration and end-to-end tests so they can be selected and excluded.

## Test doubles

- Inject doubles through the seam the code already has: a constructor, a parameter. Don't patch the module the code imports from. The stack skill names the exceptions.
- A double is bound to the real interface, so a renamed method or a changed signature fails the test instead of passing silently.

## Structure

Arrange, act, assert:

- One action per unit or integration test. Two acts means two tests.
- An end-to-end test is one user flow: several actions, split into named steps.
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

- **Every source file maps to one predictable test location.** The stack skill or the project says where; deviate only for a strong, explicit reason.
- **A source file whose tests outgrow one file** gets a test directory of the same name, split by concern. Never split into sibling files with different names: that breaks the source-to-test mapping.
- **Hoist duplicated fixtures and helpers** to the nearest shared place that covers all their users, and no higher.
- **Integration tests** are grouped by technology, then by functionality. **End-to-end tests** by scenario or user flow.
- **Tests never import from other test files.** Shared helpers live in a dedicated place the stack skill names.
- Keep test files under 300 lines, unless the project sets its own limit. Split as above.

## Reviewing

1. Find the test files changed on the branch (diff against the default branch).
2. For every changed source file, look for the matching test change. Behavior that was added or changed without a test is a finding, not a note. So is a test that tests nothing real: propose deleting it.
3. Check the changed tests against every section above, the stack skill, and the project's own testing conventions where it has them.
4. Report each finding as file and line, severity, and what to change.

> 🐣 _Test data (names, countries, dates) can be fun. Easter eggs and jokes are welcome._

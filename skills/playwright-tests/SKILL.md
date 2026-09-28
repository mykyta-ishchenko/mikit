---
name: playwright-tests
description: Use when writing or reviewing browser end-to-end tests: "write an e2e test for this flow", "add a Playwright test", "review these e2e tests".
---

# Playwright Tests

**REQUIRED BACKGROUND:** load `mikit:better-tests` first. It holds the rules for every stack; this skill adds Playwright end-to-end tests on top.

If the project already runs its browser tests on Cypress or another runner, follow that runner and don't propose a migration.

## Test data

- **Each test owns the data it changes.** A fixture creates it through the API and deletes it in teardown, so cleanup runs even when the test fails: its own user when the test changes a cart, orders, or settings; its own coupon, product, or record when the test depends on one.
- **Shared data is read-only.** A seeded catalog is fine. A shared account whose state the test changes is not: parallel workers and concurrent CI runs collide on it, and emptying it first doesn't help.
- **Names are unique per run**: suffix them with a random id.

## Environment and config

- Required settings (base URL, credentials) come from the environment and fail the run at startup when missing. The base URL defaults to localhost only when the config also starts the app with `webServer`.
- Sign in through the API, once per worker or per test, and hand the session to the page with `storageState`. The login form gets its own test.
- Record traces for failures: `trace: 'on-first-retry'` when CI retries, `'retain-on-failure'` otherwise.

## Locators and waiting

- Locate by role and accessible name, then label, then text. A test id only when nothing visible identifies the element; never CSS or XPath tied to markup.
- Assert with web-first assertions: `await expect(locator).toBeVisible()`, `toHaveText`, `toHaveURL`. Never `waitForTimeout`, never a one-shot `isVisible()` check.
- Don't mock your own backend with `page.route`. A test against a mocked backend is a component test and belongs in the unit suite.

## Organization

- One test per user flow, with `test.step` naming each stage.
- Shared fixtures live in one fixtures module that every spec imports `test` and `expect` from.

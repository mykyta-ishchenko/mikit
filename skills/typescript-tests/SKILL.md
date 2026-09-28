---
name: typescript-tests
description: Use when writing or reviewing TypeScript, JavaScript, React, or Astro unit and component tests: "write tests for this component", "review these Vitest tests".
---

# TypeScript Tests

**REQUIRED BACKGROUND:** load `mikit:better-tests` first. It holds the rules for every stack; this
skill adds TypeScript, Vitest or Jest, and React Testing Library on top.

## What to test in TypeScript

| Test this                                                           | Don't test this                                                      |
|---------------------------------------------------------------------|----------------------------------------------------------------------|
| Your conversion before `Intl` is called: cents to units, rounding   | `Intl.NumberFormat` output for other currencies, locales, separators |
| A type guard or parser that runs at runtime                         | What the type checker already enforces                               |
| What a component renders in each state: loading, empty, error, data | That React re-renders, or that a prop reaches a child                |

Parametrize with `it.each`.

## Mocking

- Components and hooks: intercept the network with MSW, not `vi.mock` of the API module. One server
  with happy-path handlers starts in the test setup file with `onUnhandledRequest: 'error'` and
  resets after each test; a test overrides a handler with `server.use`.
- Everything else: inject dependencies through parameters or constructors.
- `vi.mock` only for a third-party module the code imports directly with no network to intercept:
  analytics, feature flags, error reporting.
- A framework's virtual modules (`astro:env/client`, SvelteKit's `$env/static/public`) are not that
  exception. Give the function a parameter that defaults to the imported value and pass each case
  in the test: no `vi.mock`, `vi.doMock`, `vi.stubEnv`, or re-import after `vi.resetModules`.
- Type every double against the real signature: `vi.fn<typeof fetchCart>()`, or an object that
  `satisfies` the interface.

## React Testing Library

- Query as a user finds things: `getByRole` with a name, then `getByLabelText`, then `getByText`.
  `getByTestId` only when nothing visible identifies the element; never `container.querySelector`.
- Drive it with `userEvent.setup()`, not `fireEvent`.
- Wait with `findBy*`. No manual `act`, no side effects inside `waitFor`.
- Assert what the user sees and what the server received, never component state, props, or class
  names.
- No snapshots in place of assertions.

## Astro

An adapter that runs the app in another runtime, such as Cloudflare's workerd, stops Vitest at
startup with `module is not defined`. The inline config argument of `getViteConfig()` can't remove
it: leave the adapter out of the Astro config when `process.env.VITEST` is set.

## Organization

- **Tests sit next to the source**: `src/lib/money.ts` is tested by `src/lib/money.test.ts`, unless
  the project keeps them elsewhere.
- **Shared helpers live next to the test setup file**: factories, render helpers, the MSW server and
  its handlers. Never in a `*.test.ts` file.

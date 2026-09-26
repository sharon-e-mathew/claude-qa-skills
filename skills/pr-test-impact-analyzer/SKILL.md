---
name: pr-test-impact-analyzer
description: Look at a pull request and work out what needs testing — which areas are affected, which existing tests to run, and what test coverage is missing. Use for "what should I test in this PR", "what does this PR affect", "test impact of this change", or "is this PR covered by tests".
author: Sharon Mathew
version: 1.0.0
---

# PR Test Impact Analyzer

Goal: before I test a PR, know **what changed, what it could break, and what tests cover it** — so I test the right things, not everything.

## When to use

- A PR is ready for QA
- I want to know which e2e or unit tests to run
- I want to check if a change has enough tests

## Step 1 — See what changed

Get the list of changed files from the PR (for example with `gh pr diff <number> --name-only`, which only reads the PR and changes nothing).

Sort them:
- **Code** — components, pages, API, database, shared libraries
- **Tests** — spec/test files
- **Config** — package files, CI, env, feature flags
- **Other** — docs, translations, images

## Step 2 — Work out what's affected

For each code file:
1. What does it do? (page, component, API, helper)
2. Who uses it? Search for places that import it.
3. Is it **shared**? A change in a shared library (in a monorepo, often under `libs/` or `packages/`) can affect several apps. Say which ones.

In an Nx monorepo, Nx can list affected projects:
```bash
npx nx show projects --affected
```
This only reads the code and prints project names.

## Step 3 — Rate the risk

| Risk | Signs |
|---|---|
| **High** | Login/permissions, payments, data saving/deleting, database changes, shared libraries, big changes with no tests |
| **Medium** | A main user flow, API changes, forms and validation |
| **Low** | Text, styling, docs, small isolated change with tests |

## Step 4 — Find the existing tests

For each changed file, look for:
- A test next to it (`Foo.tsx` → `Foo.spec.tsx` or `Foo.test.tsx`)
- e2e tests that visit the affected pages
- Tests that import the changed code

List the exact commands to run them, and explain what each command does.

## Step 5 — Spot the gaps

Flag:
- New or changed logic with no test added
- New error paths or conditions (`if` branches) that aren't tested
- A bug fix with no test that would catch it coming back
- Tests that were deleted or skipped

## Step 6 — Give me a test plan

```
**PR:** #123 — short title
**Risk:** High / Medium / Low — one-line reason
**Affected areas:** patient portal, shared billing library
**Run these tests:**
- command — what it covers
**Manual checks:**
1. ...
**Regression checks (nearby things that could break):**
- ...
**Missing tests:**
- ...
**Questions for the dev:**
- ...
```

Order the checks with the riskiest first.

## Mistakes to avoid

- Only testing the file that changed, not what uses it
- Ignoring config or database changes
- Assuming "CI passed" means it's well tested
- Running the whole test suite when a few targeted tests would do (or the opposite for high-risk changes)

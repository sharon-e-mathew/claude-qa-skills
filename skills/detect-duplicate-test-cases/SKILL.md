---
name: detect-duplicate-test-cases
description: Find test cases that are duplicates, near-duplicates, or covered by another test — in manual (markdown, Gherkin) and automated (Playwright, Jest, Cypress) suites — and suggest which to keep, merge, or remove. Use for "find duplicate tests", "clean up the test suite", "which tests overlap", or "are these tests the same".
author: Sharon Mathew
version: 1.0.0
---

# Detect Duplicate Test Cases

A bigger test suite isn't a better one. Duplicate tests cost time to run, time to maintain, and hide what's really covered. This skill finds them and suggests a clean-up — **nothing changes until I approve it.**

## Step 1 — Collect the tests

Look for:
- **Manual:** markdown test files (often in `tests/`, `manual-tests/`, `docs/tests/`), Gherkin `.feature` files
- **Automated:** `*.spec.ts`, `*.test.ts`, `*.test.js`, `*.cy.ts` and similar

If I name a folder or suite, only look there.

For each test, note: ID (if any), title, steps, expected results, tags, preconditions, and file location.

## Step 2 — Compare what each test is really checking

Before comparing, even out the surface differences:
- Lowercase, ignore punctuation and formatting
- Ignore framework words (`describe`, `it`, `test`, `Scenario`, `Given/When/Then`)
- Treat test data as a placeholder — `user1@test.com` and `user2@test.com` are the same thing

Then compare pairs on: title, steps, expected results, preconditions, tags, and where the test lives.

## Step 3 — Sort each match into a level

| Level | What it means | Usual action |
|---|---|---|
| **1. Exact duplicate** | Same steps and same checks, maybe different title | Remove one |
| **2. Same intent** | Worded differently but tests the same thing the same way | Merge |
| **3. Subset** | Test A's steps and checks are all inside test B | Remove A, or keep if A is a fast smoke test |
| **4. Small variation** | Same flow, only the data changes (e.g. 5 tests that differ by one input) | Merge into one data-driven test, or keep if each value is a real boundary |

Give each pair a rough similarity score (for example 95%) and a one-line reason.

**Not duplicates** (keep both):
- Same flow on a different role, browser, device, or environment on purpose
- One is manual, one is automated, and the team wants both
- Each covers a different boundary or error case

## Step 4 — Report

```
**Scanned:** 240 tests in 18 files
**Found:** 6 exact, 9 same-intent, 4 subset, 3 variation groups

### Group 1 — Level 2, ~90% similar
- TC-014 "Login with valid email" — tests/manual/login.md
- TC-087 "User can sign in" — tests/manual/auth.md
**Why:** same steps and check, different wording
**Suggest:** merge into TC-014, keep tags from both
```

Then list all suggested actions in one place and **ask me to approve** — I can approve all, some, or none.

## Step 5 — Apply only what I approve

**Merge:**
- Keep the main test's ID; note the merged IDs in it ("Merged from TC-087")
- Combine tags and keep any unique steps or checks from both
- Keep the file's format (markdown, Gherkin, `describe/it`)

**Remove:**
- Only if the kept test really covers everything the removed one did
- Add a note in the kept test saying what was removed
- Never remove a test that has unique steps, checks, tags, or ticket links

**Keep both:**
- Add a short note to each explaining why both exist

## Rules I never break

- Don't change or lose test IDs
- Don't drop links to tickets, requirements, or Linear issues
- Don't break the test framework's syntax
- Don't move files or change folder structure
- For automated tests, run them after merging to make sure they still pass

## Done when

- I have a clear report of duplicate groups with reasons
- Only the actions I approved have been applied
- Coverage is the same or better than before, with fewer tests

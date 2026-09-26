---
name: manual-test-case-generator
description: Write a manual test case sheet (CSV) for a feature, page, or file by reading the actual code. Use for "write test cases for X", "test case sheet for this feature", or "manual tests for this file". Not for writing automated test scripts.
author: Sharon Mathew
version: 1.0.0
---

# Manual Test Case Generator

Goal: a test case sheet I can import into my test tool (for example Testomat.io or a spreadsheet), based on what the code **actually does** — not on guesses.

## Rules I always follow

1. **Read the code first.** Find the component, page, API, and validation rules before writing a single case.
2. **Only test what exists.** If the code doesn't do it, don't write a case for it. If something is unclear, list it as a question.
3. **Every case must be runnable by someone new.** Clear steps, clear expected result, any test data included.
4. **One thing per case.** If a case checks two things, split it.
5. **Cover more than the happy path.** Include negative, edge, and boundary cases. If the feature touches login, permissions, forms, payments, or external services, add at least one security-style negative case (wrong role, expired session, bad input) — but only if the code suggests it's relevant.
6. **Point to the code.** Each case notes the file it's based on, so it can be checked later.

## Steps

1. Ask (or work out) which feature, page, or file to cover.
2. Find the related code: UI components, form validation, API calls, permission checks, error messages.
3. List the things a user can do and the rules the code enforces (required fields, max lengths, roles, states).
4. Write cases in this order:
   - Happy path (main flow works)
   - Validation (required, format, length limits)
   - Boundaries (min, max, just over, just under)
   - Permissions (each role that matters)
   - Error handling (network fail, server error, empty states)
   - UI states (loading, empty, long text, mobile width)
5. List open questions at the end — things the code doesn't make clear.
6. Save as CSV and give me a short summary: how many cases, by type, and any gaps.

## CSV columns (in this order)

| Column | What goes in it |
|---|---|
| ID | TC-001, TC-002, … |
| Title | Short, starts with a verb: "Save patient record with empty date of birth shows error" |
| Area | Feature or page |
| Type | Positive / Negative / Edge / Boundary / Security / UI |
| Priority | High / Medium / Low |
| Preconditions | Account, role, data that must exist first |
| Steps | Numbered steps in one cell |
| Test Data | Exact values to enter |
| Expected Result | What should happen — specific, checkable |
| Source | File path the case is based on |

## Mistakes to avoid

- Writing cases for features that don't exist in the code
- Vague expected results like "works correctly"
- Only writing happy-path cases
- Steps that assume knowledge only the writer has

---
name: qa-split-testing-levels-pyramid
description: Decide which level each test for a feature belongs at — unit, integration, e2e, or manual — using the test pyramid, and show how the tests split across levels. Use for "unit, integration or e2e?", "what level should this test be?", "how should we test this feature?", "what coverage do we need?", or "apply the test pyramid".
author: Sharon Mathew
version: 1.0.0
---

# Split Tests Across Levels (Test Pyramid)

The idea: **test each thing at the cheapest level that can still catch the bug.**

- Lower levels (unit) are fast, cheap, and stable → most tests go here
- Higher levels (e2e, manual) are slow and break more easily → only the things lower levels can't catch

## What I can start from

- A feature description or ticket
- A list of risk scenarios (for example from **qa-thinking**)
- A branch or PR

## The levels

Default, from bottom to top:

| Level | Checks | Example |
|---|---|---|
| **Unit** | One function or component on its own | Transfer amount validation rejects `0` and negative numbers |
| **Integration** | A few parts working together (component + API, API + database) | Submitting a transfer calls the API and updates the balance on screen |
| **E2E** | A full user flow in a real browser or app | Log in, transfer money, see it in transaction history |
| **Manual** | Things that are hard or not worth automating | Look and feel, one-off exploratory checks, real devices |

A level can have more than one kind of test, each with its own runner — for example `e2e web`, `e2e api`, `e2e mobile`. Some projects also have component, contract, or visual tests.

**Use the levels the project actually has.** Only fall back to the default table if I can't find any.

## Steps

1. **See what already exists.** Look for test folders and file types (`*.spec.ts`, `*.test.ts`, e2e folders, Playwright config). Count the tests at each level and note what they cover.
2. **List the project's levels and kinds** based on what was found.
3. **Place each scenario** at the lowest level and kind that would catch the bug.
4. **Suggest a split**: how many tests at each level and kind.

## Rules

- Don't suggest manual checks for things automated tests already cover.
- When a scenario goes to a higher level, say **why** a lower one isn't enough (for example, "needs a real browser redirect, so unit can't catch it").
- One kind doesn't cover another at the same level: `e2e web` tests don't replace `e2e api` tests.
- Summarise the existing tests at a high level. Only list them one by one if I ask.

## Output

### 📐 Testing levels
The levels and kinds this project uses (only if they differ from the default).

### ☑️ What is already tested
Number of tests and what they cover, per level and kind.

### 🧪 Testing plan
| Scenario | Level / kind | Why this level |
|---|---|---|
| Transfer of `0.00` is rejected | unit | Pure validation logic, no UI needed |
| View-only account holder can't make a transfer | e2e / api | Needs real permission checks on the server |

For **manual** items, add how to check them: `action → expected result`.

If the output can show charts, add a simple pyramid or pie chart of test counts per level. Skip it in plain terminal output.

## After the plan

Offer the next step:
- Write the manual cases → **manual-test-case-generator**
- Add the bad-input and permission tests → **negative-test-generator**
- Check what a PR affects → **pr-test-impact-analyzer**

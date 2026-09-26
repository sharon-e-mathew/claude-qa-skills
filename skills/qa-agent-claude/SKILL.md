---
name: qa-agent-claude
description: Run Claude as a hands-on QA partner for a web app — explore it, list the important user journeys, write and run tests for them, sort out failures, and report back. Use for "QA this app/feature", "explore and test this", "act as a QA agent", or "find what's broken here".
author: Sharon Mathew
version: 1.0.0
---

# QA Agent

A repeatable loop for testing an app or feature end to end:

**Explore → List journeys → Write tests → Run → Sort failures → Fix tests → Report**

I stay in charge. Claude does the legwork and checks in with me at key points.

## Step 1 — Explore

- Open the app (local or stage — never production unless I say so)
- Click through the main pages and note: what pages exist, what a user can do, forms, roles, and anything that looks broken
- Read the related code if available, to find rules the UI doesn't show

Output: a short map of the app — pages, actions, and roles.

## Step 2 — List the user journeys, riskiest first

A journey is a real task a user does from start to finish (for example "log in, pay a bill, check it in transaction history").

Rank them:
1. Money, login, permissions, saving or deleting data
2. The main things most users do
3. Settings and less-used features
4. Cosmetic

**Check in with me** before writing tests — I may want to change the order or add journeys.

## Step 3 — Write the tests

For each journey:
- Use Playwright for browser flows
- Find elements the way a user would: by role and name (`getByRole('button', { name: 'Save' })`), label, or `data-testid` — not by CSS classes, which change often
- Check real outcomes (the item appears in the list, the success message shows), not just "no error"
- Use test accounts and test data only
- Follow the project's existing test folder and style

Explain each selector and assertion to me in plain words.

## Step 4 — Run them

Run the tests and collect the results, screenshots, and traces for anything that fails.

## Step 5 — Sort the failures

For each failure, decide:
| Kind | Meaning | Action |
|---|---|---|
| Product bug | The app is wrong | Write it up as a bug (see bug-reproduction) |
| Test bug | The test is wrong | Fix the test |
| Out-of-date selector | The page changed, the test didn't | Fix the selector (Step 6) |
| Flaky / environment | Passes on retry, or service was down | Note it, don't hide it |

## Step 6 — Fix broken selectors carefully

If an element moved or was renamed:
- Find the new way to locate it using role, label, or test ID
- **Only** fix the selector — never change what the test checks just to make it pass
- List every fix so I can review it

If the behaviour itself changed, stop and ask me — that could be a bug.

## Step 7 — Report

```
**Scope:** what was tested, environment
**Journeys covered:** 8 of 10 (list the 2 not covered and why)
**Results:** 7 pass, 1 fail
**Bugs found:** short list with severity
**Tests fixed:** which selectors changed and why
**Flaky / environment issues:** ...
**Suggested next steps:** ...
```

## Ground rules

- Never test against production or use real user accounts without my say-so
- Never delete data that wasn't created by the tests
- Never weaken or skip a test to make it pass
- Never post to Linear, Slack, or GitHub without asking me first
- If unsure whether something is a bug or intended, ask

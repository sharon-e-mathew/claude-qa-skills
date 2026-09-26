---
name: bug-reproduction
description: Reproduce a reported bug step by step before anyone tries to fix it. Use when given a ticket (Linear, Jira), a bug report, a Slack message, or a screenshot and asked to "repro", "reproduce", "check if this still happens", or "confirm this bug".
author: Sharon Mathew
version: 1.0.0
---

# Bug Reproduction

My rule: **no fix until the bug is reproduced.** A bug I can't make happen on purpose is a bug nobody can prove is fixed.

## When to use

- A ticket (for example `MED-123`) needs checking before a dev picks it up
- Someone says "this is broken" in Slack and I need to confirm it
- A fix has landed and I need to check the bug is really gone

## Step 1 — Read the report and write down what's missing

Pull these out of the ticket. If one is missing, write "unknown", don't guess:

| What | Example |
|---|---|
| Where | URL, app, page, screen |
| Who | Role or account type (admin, doctor, patient) |
| Environment | local, stage, production |
| Browser / device | Chrome 128 on macOS, iPhone 15 Safari |
| Steps | What the person clicked, in order |
| Expected | What should have happened |
| Actual | What happened instead (error text, screenshot) |
| When | First seen date, and does it happen every time? |

If the steps are vague, list the questions to ask the reporter. Don't start guessing at causes.

## Step 2 — Try to make it happen

1. Use the same environment, role, and browser as the reporter.
2. Follow their steps exactly, once.
3. Record what I see: screenshot, screen recording, console errors, failed network calls.
4. Try it **3 times**. Note how many times it happened (for example "2 out of 3").

## Step 3 — Make the steps as short as possible

Once it happens reliably, cut the steps down:

- Remove **one** step at a time and try again.
- If the bug still happens, keep that step removed.
- If it stops happening, put that step back — it matters.
- Stop when every remaining step is needed.

Changing more than one thing at once hides which one actually mattered.

## Step 4 — Narrow down the cause (optional)

Only if it helps the dev:

- **Does it happen on another browser?** → browser-specific or not
- **Does it happen with another account/role?** → permissions or data
- **Does it happen on stage but not local?** → config or data difference
- **Did it work before?** → find the last good release or commit

To find the commit that broke it, a dev (or I, with help) can use `git bisect`. It tests commits between a known-good and known-bad point to find the first bad one. It doesn't change any code, but ask before running it if unsure.

## Step 5 — Decide what kind of bug it is

| Result | What to write |
|---|---|
| Happens every time | **Reproduced** — include the short steps |
| Happens sometimes | **Intermittent** — include how often (e.g. 2/5) and anything that seems to trigger it |
| Only in one environment | **Environment-specific** — say which one and what's different |
| Can't make it happen | **Not reproduced (yet)** — list exactly what I tried. Don't close it after one try; ask the reporter for more detail first |

## Step 6 — Update the ticket

Add a comment to the ticket using this format:

```
**Repro status:** Reproduced / Intermittent / Not reproduced
**Environment:** stage, Chrome 128, macOS, patient account
**Steps (shortest):**
1. ...
2. ...
**Expected:** ...
**Actual:** ...
**How often:** 3/3
**Evidence:** screenshot / recording / console error
**Notes:** anything that narrowed it down
```

Always ask before posting to the ticket tracker or Slack.

## Step 7 — After the fix

1. Follow the same short steps on the fixed build.
2. Check the bug is gone **3 times**.
3. Check the nearby flows still work (quick regression check).
4. If there's an automated test for it, confirm it fails without the fix and passes with it.
5. Update the ticket with the result.

## Mistakes to avoid

- Starting to fix or guess the cause before it's reproduced
- Cutting steps before confirming the bug happens at all
- Closing as "can't reproduce" after one try
- Saying "fixed" because it passed once
- Testing with a different role or environment than the reporter

## Done when

- The ticket has a clear repro status, short steps, and evidence
- Anyone on the team could follow the steps and see the same thing

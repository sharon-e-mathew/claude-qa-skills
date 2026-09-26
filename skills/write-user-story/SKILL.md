---
name: write-user-story
description: Turn a feature idea, ticket, notes, or how something already works into user stories with acceptance criteria. Use for "write user stories for X", "draft acceptance criteria", "turn this into requirements", "write a spec", or "rewrite this ticket's AC".
author: Sharon Mathew
version: 1.0.0
---

# Write User Stories

A user story says **who** needs **what** and **why**. The acceptance criteria (AC) under it are the actual requirements — each one a clear rule that either passes or fails.

## What I can start from

- A feature idea or notes
- A ticket (Linear, Jira) — use the tracker tools if connected, otherwise ask me to paste it
- A PR — read the description and changes to work out what it's meant to do
- How the app works today (for writing down existing behaviour)

## Story format

```
**US-1: <short name>**
As a <who>, I want <what>, so that <why>.

Acceptance criteria:
- Given <starting point>, when <action>, then <result>. (AC-1)
- ... (AC-2)
```

- IDs are local only: `US-1`, `AC-1`. Never make up ticket or Testomat IDs.
- Put the AC ID at the **end** of the line — easier to read.
- Every story has at least one AC.

If I ask for another format (Gherkin, use cases, a BRD, ticket-style AC), use it — but still break it into stories with AC underneath.

## Every story must be

| Check | Means |
|---|---|
| **One thing** | One type of user, one ability. Split if there's an "and" |
| **Clear** | No vague words: fast, easy, relevant, user-friendly, as needed, should work |
| **Complete** | Says who, what triggers it, what happens, and the rules. AC covers empty, error, and permission cases too |
| **Consistent** | Same word for the same thing everywhere; nothing contradicts |
| **Testable** | Each AC has a clear pass or fail |

## Say what, not how

Describe what the user needs, not how it's built. Don't mention buttons, dropdowns, APIs, or frameworks unless I ask for that level of detail.

- ❌ "Show a red toast using the Snackbar component"
- ✅ "The user is told the save failed and their changes are kept"

## Never guess

If a rule is missing or unclear, **don't invent it**. List it under **Open questions** and ask me. If I had to assume something to write a story, list it under **Assumptions** so it's easy to check.

## Output

1. User stories with AC
2. **Assumptions** — anything I assumed
3. **Open questions** — what needs an answer before building

## Example

**US-1: Book a GP appointment**
As a patient, I want to book an appointment with my GP, so that I can be seen without phoning the clinic.

Acceptance criteria:
- Given my GP has a free slot, when I pick it and confirm, then the appointment shows in *My Appointments*. (AC-1)
- Given someone else books the slot while I'm confirming, when I confirm, then I'm told it's taken and shown other free slots. (AC-2)
- Given I'm not logged in, when I try to book, then I'm asked to log in first and returned to the booking afterwards. (AC-3)

Open questions:
- How far ahead can a patient book?
- Can a patient book more than one appointment on the same day?

## After writing

Offer the next step:
- Find the risks → **qa-thinking**
- Turn the AC into test cases → **qa-write-test-cases**
- Plan which test levels to use → **qa-split-testing-levels-pyramid**

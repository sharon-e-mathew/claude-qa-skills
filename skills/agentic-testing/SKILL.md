---
name: agentic-testing
description: Plan and set up testing where AI agents do the repetitive parts — picking which tests to run, drafting new tests, spotting failure patterns — while a person reviews the important decisions. Use for "set up AI-driven testing", "use agents to maintain our tests", "automate test selection", or "design an agent test pipeline".
author: Sharon Mathew
version: 1.0.0
---

# Agentic Testing

"Agentic testing" means letting AI agents handle the repetitive test work, with a person approving the decisions that matter. It's a way of working, not a single tool.

## Where agents help — and where they don't

| Good for agents | Keep a person in charge |
|---|---|
| Choosing which tests to run for a change | Deciding what "correct" behaviour is |
| Drafting tests from a journey or ticket | Approving new tests into the suite |
| Fixing selectors that moved | Changing what a test checks |
| Grouping similar failures | Deciding if a failure is a real bug |
| Writing first-draft bug reports | Filing, closing, or prioritising tickets |
| Flagging flaky tests | Deleting or skipping tests |

## The loop

1. **Look at the change** — what files changed, what they affect, how risky it is (see pr-test-impact-analyzer)
2. **Pick tests** — run the most relevant and riskiest first, not the whole suite every time
3. **Fill gaps** — draft tests for changed code that has none
4. **Run** — collect results, screenshots, traces
5. **Sort failures** — product bug, test bug, flaky, or environment (see ai-bug-triage)
6. **Review** — a person approves new tests, fixes, and tickets
7. **Learn** — keep a simple record of what failed and why, so the next run picks better

## Split the jobs

It works better with separate focused roles than one agent doing everything:
- **Planner** — reads the change, decides what to test
- **Writer** — drafts or updates tests
- **Runner** — runs them and gathers results
- **Reviewer** — sorts failures and writes the summary

These can be separate Claude sessions or subagents. Each one hands a short, clear summary to the next.

## Setting it up (start small)

1. Start with **one** job — usually test selection or failure sorting
2. Run it alongside the normal process for a few weeks and compare
3. Measure: time saved, bugs caught, false alarms, tests the agent got wrong
4. Only then add the next job

## What to track

- How often the agent's chosen tests caught the bug (vs running everything)
- How many drafted tests were accepted without big changes
- How many "fixed" tests were actually hiding a real bug
- Flaky test count over time

## Guardrails

- Agents never delete or skip tests on their own
- Agents never change what a test checks — only how it finds things
- Agent-written tests can be wrong; check them against known-good behaviour
- Anything posted to Linear, Slack, or GitHub needs a person's OK
- Keep a log of what the agent changed and why

## Mistakes to avoid

- Trusting agent-written tests without review
- Letting the agent "fix" tests until they pass
- Automating everything at once
- No record of what the agent did, so no one can check it

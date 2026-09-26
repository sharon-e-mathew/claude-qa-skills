# Claude QA Skills

A set of [Claude Code](https://claude.com/claude-code) skills I built for my day-to-day QA work: reproducing bugs, writing and cleaning up test cases, planning test coverage, and reviewing features and pull requests from a tester's point of view.

Each skill is a plain markdown file that tells Claude how I want a QA task done: the steps to follow, the format to use, and when to stop and check with me.

## The skills

### Planning and requirements
| Skill | What it does |
|---|---|
| [write-user-story](skills/write-user-story/SKILL.md) | Turns an idea, ticket, or notes into user stories with testable acceptance criteria |
| [qa-thinking](skills/qa-thinking/SKILL.md) | Reviews a feature as QA: what to acknowledge, what to clarify, what to verify |
| [qa-split-testing-levels-pyramid](skills/qa-split-testing-levels-pyramid/SKILL.md) | Decides whether each scenario belongs in unit, integration, e2e, or manual testing |
| [pr-test-impact-analyzer](skills/pr-test-impact-analyzer/SKILL.md) | Works out what a pull request affects and what needs testing |

### Test cases
| Skill | What it does |
|---|---|
| [qa-write-test-cases](skills/qa-write-test-cases/SKILL.md) | Guided checklist → test cases flow, with approval at each step and Testomat.io support |
| [manual-test-case-generator](skills/manual-test-case-generator/SKILL.md) | Builds a CSV test case sheet from the actual source code |
| [negative-test-generator](skills/negative-test-generator/SKILL.md) | Finds how a form or API can be broken: bad input, limits, permissions |
| [improve-test-cases](skills/improve-test-cases/SKILL.md) | Tidies existing manual test cases without changing their IDs or intent |
| [detect-duplicate-test-cases](skills/detect-duplicate-test-cases/SKILL.md) | Finds duplicate and overlapping tests and suggests keep / merge / remove |

### Bugs
| Skill | What it does |
|---|---|
| [bug-reproduction](skills/bug-reproduction/SKILL.md) | Reproduces a reported bug, cuts it down to the shortest steps, and writes up the evidence |
| [ai-bug-triage](skills/ai-bug-triage/SKILL.md) | Groups duplicates, sets severity and priority, and drafts clean tickets |

### Automation
| Skill | What it does |
|---|---|
| [qa-agent-claude](skills/qa-agent-claude/SKILL.md) | Explore → write tests → run → sort failures → report loop for a web app |
| [playwright-visual-ai](skills/playwright-visual-ai/SKILL.md) | Stable Playwright screenshot tests and how to review visual diffs |
| [agentic-testing](skills/agentic-testing/SKILL.md) | How to let AI agents handle repetitive test work, with people approving key decisions |

## How I use them

Every skill follows the same few rules:
- **Check with me first.** Nothing gets posted, filed, deleted, or uploaded without my OK.
- **Base it on real evidence.** Read the code, the ticket, or the app. Don't guess.
- **Keep it plain.** Short steps, specific expected results, no vague words like "works correctly".

## Install

Claude Code loads personal skills from `~/.claude/skills/`.

**Option 1: link them.** Edits in this repo show up in Claude straight away:

```bash
git clone https://github.com/sharon-e-mathew/claude-qa-skills.git
```

```bash
cd claude-qa-skills && for d in skills/*/; do ln -s "$PWD/$d" ~/.claude/skills/; done
```

**Option 2: copy one skill:**

```bash
cp -R skills/bug-reproduction ~/.claude/skills/
```

Then in Claude Code, type `/bug-reproduction` (or just describe the task, and Claude picks the right skill).

## License

MIT, see [LICENSE](LICENSE).

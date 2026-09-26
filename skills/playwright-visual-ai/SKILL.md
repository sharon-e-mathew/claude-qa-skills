---
name: playwright-visual-ai
description: Set up and review visual (screenshot) tests with Playwright to catch layout and styling changes. Use for "visual test for this page", "screenshot comparison", "catch UI regressions", "update the baseline screenshots", or "why is this visual test failing".
author: Sharon Mathew
version: 1.0.0
---

# Playwright Visual Testing

Visual tests take a screenshot and compare it to a saved "baseline" (the approved picture). If the pixels differ too much, the test fails. They catch things normal tests miss: broken layout, wrong colours, missing icons, overlapping text.

## When to use

- A page or component looks wrong after a change and I want to stop it happening again
- A redesign or CSS change needs checking across screen sizes
- A visual test is failing and I need to know if it's a real change

## Key Playwright pieces (explained)

- `await expect(page).toHaveScreenshot('name.png')` — takes a screenshot of the page and compares it with the saved baseline. First run creates the baseline.
- `await expect(locator).toHaveScreenshot()` — same, but for one element only (more stable than the whole page).
- `mask: [locator]` — covers parts of the page with a box so changing content (dates, avatars, ads) doesn't cause failures.
- `maxDiffPixelRatio: 0.01` — allows up to 1% of pixels to differ, for tiny rendering noise.
- `animations: 'disabled'` — stops animations so screenshots are the same each run.
- `npx playwright test --update-snapshots` — replaces the baselines with new screenshots. Only run this when the change is **intended and approved**.

## Step 1 — Pick what to screenshot

Good choices: key pages, important components (buttons, cards, nav), empty/error states, mobile layouts.
Avoid: pages full of live data, videos, maps, or anything that changes every load.

## Step 2 — Make the screenshot stable

Flaky visual tests are worse than none. Before taking the screenshot:
1. Wait for the page to finish loading (fonts, images, network)
2. Turn off animations
3. Mask changing content — dates, times, usernames, random images
4. Use fixed test data, not live data
5. Use a fixed screen size

## Step 3 — Example test

```ts
import { test, expect } from '@playwright/test'

test('account card looks right', async ({ page }) => {
  await page.goto('/accounts')
  // wait until the card is on the page
  const card = page.getByTestId('account-card').first()
  await expect(card).toBeVisible()

  await expect(card).toHaveScreenshot('account-card.png', {
    animations: 'disabled',
    mask: [
      card.getByTestId('balance'), // hide the balance, it changes
      card.getByTestId('updated-at') // hide the date, it changes
    ],
    maxDiffPixelRatio: 0.01
  })
})
```

Why it's written this way:
- `getByTestId` finds the element by a `data-testid` attribute, which doesn't change when text or styling does
- `toBeVisible()` first makes sure the card has loaded before the screenshot
- The balance and date are masked because they would differ every run

## Step 4 — Screen sizes and browsers

Test at least a desktop and a mobile size. Use Playwright "projects" in the config for each size or browser. Baselines are saved per browser and per OS, so screenshots from a Mac won't match ones taken in CI on Linux — generate baselines in the same place the tests run (usually CI or Docker).

## Step 5 — When a visual test fails

Open the report (`npx playwright show-report`). It shows the expected, actual, and a diff image.

Then decide:
| What I see | What to do |
|---|---|
| Real unintended change | It's a bug — report it |
| Intended change (redesign) | Get it approved, then update the baseline |
| Tiny noise (anti-aliasing, fonts) | Mask it or adjust the threshold slightly |
| Random content changed | Mask it or use fixed test data |

Never update baselines just to make the test go green.

## Paid visual tools

Tools like Applitools or Percy use smarter comparison and a review dashboard. Only suggest them if the team already uses one — built-in Playwright screenshots are enough to start.

## Mistakes to avoid

- Whole-page screenshots of pages with live data
- Updating baselines without checking the diff
- Making baselines on a laptop and running tests in CI
- Setting the threshold so high real changes slip through
- Too many visual tests — keep them for things that really matter

---
name: pmo-ux-review
description: Use when reviewing or critiquing any screen, mockup or UI change for the IT PMO board.
---

# PMO UX Review

Before reviewing, look at the actual screen: read the relevant code
(e.g. `index.html`) and, where possible, capture screenshots at desktop
(1440px) and mobile (375px) widths with headless Chromium/Playwright.
Base every finding on what you observed, citing the element or line.

Review the screen against each check. For every issue,
give: what's wrong, why it matters, and a specific fix.

## 1. Hierarchy
- Is the most urgent item (Overdue, Critical) seen first?
- Does each screen have one clear primary action?

## 2. Clarity
- Dates shown as relative ("Due in 3 days") with exact date on hover
- Status and priority never shown by color alone (add icon or text)

## 3. Error prevention
- Destructive actions (delete, close) need confirmation
- Delete is never the most prominent control on a card

## 4. Accessibility
- Text contrast at least 4.5:1
- Every action works by keyboard, with a visible focus state
- Touch targets at least 44×44px

## 5. Mobile
- Usable at 375px width with no sideways scrolling
- Board columns become swipeable tabs

## Output format
1. Top 3 issues (highest impact first)
2. Full checklist results (Pass / Fix)
3. Quick wins: changes under 1 hour of effort

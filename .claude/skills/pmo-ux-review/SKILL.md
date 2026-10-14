---
name: pmo-ux-review
description: Use when reviewing or critiquing any screen, mockup or UI change for the IT PMO board, or when building new UI for it. Contains the board's design system (colours, type, spacing, components, mobile rules) and the UX review checklist.
---

# PMO UX Review

Before reviewing, look at the actual screen: read the relevant code
(e.g. `index.html`) and, where possible, capture screenshots at desktop
(1440px) and mobile (375px) widths with headless Chromium/Playwright.
Base every finding on what you observed, citing the element or line.

Review the screen against each check below, including conformance to the
design system. For every issue, give: what's wrong, why it matters, and a
specific fix. When building new UI, use only the values in the design system.

---

# Design system

The source of truth is the `:root` block in `index.html`. Use the CSS
variable, never a raw hex, in code. If a value here and the code disagree,
flag it as a finding.

## Colours

### Brand and surfaces

| Role | Token | Hex | Use |
|---|---|---|---|
| Brand darkest | `--c-brand-900` | `#0b2545` | Header gradient start, modal headings, inner focus ring |
| Brand dark | `--c-brand-700` | `#13427a` | Header gradient end, count badges, selected tab, tag text |
| Brand hover | `--c-brand-600` | `#1a56a0` | Primary button hover |
| Brand | `--c-brand-500` | `#2468c0` | Primary button, drop-target border, info toast edge |
| Brand tint | `--c-brand-100` | `#e4eefa` | Light button hover, tag border |
| Brand wash | `--c-brand-50` | `#f3f7fd` | Tag background, ghost hover, modal header, move menu |
| Page background | `--c-bg` | `#eef2f7` | `body` |
| Column background | — | `#e3e9f1` | Board columns |
| Surface | `--c-surface` | `#ffffff` | Cards, filter bar, modal, tabs |
| Border | `--c-border` | `#d4dce7` | Cards, inputs, ghost buttons |
| Drop target | `--c-drop` | `#d6e6fb` | Column while a card is dragged over it |

### Text

| Role | Token | Hex | Contrast |
|---|---|---|---|
| Primary text | `--c-text` | `#1b2533` | 13.7:1 on page bg |
| Muted text | `--c-text-muted` | `#5a6778` | 5.8:1 on white, 4.7:1 on column bg (the lowest allowed) |
| Text on dark header | — | `#ffffff` | Secondary header text may drop to 80–85% opacity, no lower |

### Accents and feedback

| Role | Token | Hex | Use |
|---|---|---|---|
| Success | `--c-success` | `#1e7d45` | Success toast edge |
| Warning | `--c-warning` | `#a35b00` | "Due soon" text (≤ 7 days), warning toast edge |
| Danger | `--c-critical` | `#c62828` | Errors, overdue text, delete confirmation |
| Focus outer | `--c-focus` | `#ffb703` | 6px outer focus ring (reads on dark) |
| Focus inner | `--c-focus-inner` | `#0b2545` | 3px inner focus ring (reads on light) |

### Priority colours

Shown as a pill (text + tinted background) **and** the card's 5px left edge.
The pill always carries the word, so colour is never the only signal.

| Priority | Text / edge | Pill background | Contrast |
|---|---|---|---|
| Critical | `#c62828` `--c-critical` | `#fdecec` `--c-critical-bg` | 4.9:1 |
| High | `#8a5300` `--c-high` | `#fff4e0` `--c-high-bg` | 5.8:1 |
| Medium | `#2468c0` `--c-medium` | `#e4eefa` `--c-medium-bg` | 4.7:1 |
| Low | `#555f6d` `--c-low` | `#eef0f3` `--c-low-bg` | 5.7:1 |

### Status colours

Use for status chips, column-header accents and tab markers. Status colours
must not reuse a priority colour for a different meaning (hence Blocked is
purple, not amber or red). Overdue is a condition, not a column: it overlays
any status except Done.

| Status | Text / accent | Background | Contrast | Icon |
|---|---|---|---|---|
| Backlog | `#555f6d` | `#eef0f3` | 5.7:1 | ○ |
| In Progress | `#1a56a0` | `#e4eefa` | 6.2:1 | ◐ |
| Blocked | `#6a3fa0` | `#f1ebfa` | 6.4:1 | ⛔ |
| Done | `#186a3b` | `#e6f4ec` | 5.9:1 | ✓ |
| Overdue | `#c62828` | `#fdecec` (border `#f5c2c2`) | 4.9:1 | ⚠ |

Status is always labelled with text (or an icon plus visually-hidden text).
Overdue cards also get a `#fffafa` card tint. Backlog, In Progress, Blocked
and Done are not yet defined as tokens in `index.html`. Add them as
`--c-status-*` variables before using them.

## Typography

- **Font:** `system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif` (`--font`)
- **Mono (task IDs only):** `ui-monospace, SFMono-Regular, Menlo, Consolas, monospace` (`--font-mono`)
- **Base:** 14px, line-height 1.45
- Never go below 11px. Weights in use: 400, 600, 700 and 800 (wordmark only).

| Element | Size | Weight | Notes |
|---|---|---|---|
| Wordmark (H1) | 22px | 800 | letter-spacing 0.04em |
| Wordmark subtitle | 14px | 400 | 80% opacity |
| Modal heading | 17px | 700 | `--c-brand-900` |
| Column heading (H2) | 15px | 700 | |
| Card title (H3) | 14px | 700 | |
| Filter heading | 14px | 700 | |
| Body / inputs / buttons | 14px | 400 / 600 (buttons) | |
| Card description, meta, field labels | 12px | 400 (labels 600) | Muted colour for description and labels |
| Column hint, pill, tag, char count | 11px | 400 / 700 (pills) / 600 (tags) | |
| Task ID | 11px mono | 400 | Muted |
| **Numbers:** summary value | 20px | 700 | Label above: 11px uppercase, 0.06em tracking |
| **Numbers:** count badge | 12px | 700 | White on `--c-brand-700`, pill shape |
| **Numbers:** tab count | 11px | 700 | |

## Spacing, radius, shadows

- **Spacing scale (4px base):** `--space-1` 4px · `--space-2` 8px ·
  `--space-3` 12px · `--space-4` 16px · `--space-5` 24px · `--space-6` 32px.
  Use only these values. The only exceptions are 1–2px hairlines and the
  6px input padding.
- **Corner radius:** `--radius-sm` 4px for buttons, inputs and menus ·
  `--radius-md` 8px for cards, filter bar, summary tiles and toasts ·
  `--radius-lg` 12px for columns and the modal · 999px for pills, tags and
  count badges.
- **Shadows:**
  - `--shadow-card`: `0 1px 2px rgba(11,37,69,0.08), 0 1px 4px rgba(11,37,69,0.06)`, for cards only
  - `--shadow-modal`: `0 12px 40px rgba(11,37,69,0.3)`, for the modal and toasts
  - No other shadows. Use borders for separation.
- **Touch target:** `--tap` 44px minimum height and width for every interactive element.

## Components

**Card**
- White surface, 1px `--c-border`, 5px left edge in the priority colour, 8px radius, 12px padding, `--shadow-card`.
- Order: task ID + priority pill (+ Overdue pill) → title → description → meta grid (Project / Assignee / Due) → footer with category tag and Move button.
- The delete "×" is a 44×44px muted icon button in the top-right corner. It must never be the most prominent control.
- Due date is relative ("Due in 5 days", "2 days overdue"), with the exact date in `title` and visually-hidden text.

**Badges**
- **Priority pill:** 11px/700, 999px radius, `1px 8px` padding, colours from the priority table.
- **Overdue pill:** same shape, `#c62828` on `#fdecec`, 1px `#f5c2c2` border, ⚠ icon + "Overdue".
- **Category tag:** 11px/600, `--c-brand-700` on `--c-brand-50`, 1px `--c-brand-100` border.
- **Count badge:** 12px/700, white on `--c-brand-700`, min-width 26px.

**Buttons** (all at least 44px tall, 4px radius, 600 weight, `0 16px` padding)
- **Primary:** white on `--c-brand-500`, hover `--c-brand-600`. One per screen or dialog.
- **Light (on the dark header):** `--c-brand-900` on white, hover `--c-brand-100`.
- **Ghost / secondary:** transparent, 1px `--c-border`, hover `--c-brand-50`.
- **Destructive confirm:** "Yes" is white on `#c62828`. "No" is outlined and gets focus by default.
- **Focus (all elements):** 3px `#0b2545` outline + 6px `#ffb703` ring.

**Filters**
- White bar, 1px border, 8px radius, `12px 16px` padding, wraps with a 12px gap.
- Label above each field: 12px/600 muted. Inputs: 44px min height, 1px border, 4px radius, 160px min width on desktop.
- "Clear filters" is a ghost button. A live "Showing X of Y tasks" count is right-aligned.
- The Overdue summary tile is a toggle (`aria-pressed`) and counts as a filter that Clear resets.

## Mobile layout rules

- **Breakpoints:** ≥ 1101px shows 4 columns · 768–1100px shows a 2×2 grid · ≤ 767px is the phone layout.
- **Phone layout:**
  - 12px side gutters.
  - No horizontal page scroll at 375px.
  - The board is a swipeable row of full-width columns (scroll-snap) with a sticky tab bar: one 44px tab per column, showing its count and ⚠ overdue count.
  - The selected tab is white on `--c-brand-700`.
  - Tabs follow swipes, and arrow keys move between tabs.
- **Header on phones:** the summary tiles wrap, with Overdue first. "Add Task" stays above the fold.
- **Filters on phones:** fields go full width and wrap (`flex: 1 1 140px`), and the count drops under the fields.
- **Modal on phones:** one-column form, width `min(560px, 100vw − 32px)`.
- **Motion:** honour `prefers-reduced-motion`, so tab scrolling is instant and transitions are off.

---

# Review checklist

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

## 6. Design system conformance
- Colours, priority and status colours match the tables above (no stray hex values)
- Type sizes and weights match the typography table, with nothing under 11px
- Spacing, radius and shadows use only the defined tokens
- Cards, badges, buttons and filters follow the component specs

## Output format
1. Top 3 issues (highest impact first)
2. Full checklist results (Pass / Fix)
3. Quick wins: changes under 1 hour of effort

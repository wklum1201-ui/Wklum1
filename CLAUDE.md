# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository contents

- `index.html` — the main project: a single-file IT PMO Kanban board (internal demo/training tool for a fictitious bank).
- `HelloWorld.java` — an unrelated standalone Java hello-world (`javac HelloWorld.java && java HelloWorld`; `*.class` is git-ignored).

There is no package manager, build step, linter config or test suite.

## Running the Kanban board

Open `index.html` directly in a browser (double-click / `file://`). It must keep working that way — no server required. If the FormSubmit call is blocked from `file://`, serve the folder with `python3 -m http.server 8000` and open `http://localhost:8000/index.html`.

Headless smoke testing: Playwright is installed globally under Node 22. Run scripts with `NODE_PATH=/opt/node22/lib/node_modules node script.js` and load `file:///home/user/Wklum1/index.html`. Stub the network with `page.route('**/formsubmit.co/**', ...)` so tests don't hit the real endpoint.

## Hard constraints on `index.html` (from the original spec — keep them)

- Vanilla HTML/CSS/JS only, all in one file (`<style>` + `<script>`). No frameworks, CDNs, web fonts, image files, npm or bundler. Icons are Unicode glyphs or inline SVG.
- **No persistence**: no localStorage, sessionStorage, IndexedDB or cookies. A refresh resetting to seed data is intended, and the header note says so.
- The only backend is FormSubmit via its AJAX JSON endpoint (`fetch`, never a navigating form POST). A FormSubmit failure must never break the board.
- Neutral "IT PMO" wordmark and corporate blue palette. Do not add real UOB logos/trademarks or imitate official UOB systems. (Task IDs `UOB-ITPM-####` and the email subject prefix were explicitly requested.)
- CSS uses custom properties on `:root` for palette/spacing; no `!important`.
- Accessibility: visible focus rings, `aria-label` on icon-only buttons, `aria-live="polite"` toast region, priority conveyed by text as well as colour.

## Architecture of `index.html`

- **Config at top of the script**: `FORMSUBMIT_ENDPOINT` (the only place the email address lives), preceded by an HTML comment explaining FormSubmit's one-time activation email. Reference data (`STATUSES`, `PROJECTS`, `CATEGORIES`, `PRIORITIES`, `ID_PREFIX`) drives both the columns and all `<select>` options — add values there rather than in markup.
- **Single source of truth**: `state = { tasks, filters, nextId, ui }`. `ui` holds transient view state (`pendingDeleteId`, `moveMenuId`, `draggingId`), so the inline "Delete? Yes / No" confirm and the "Move ▸" menu are rendered from state, not toggled in the DOM.
- **Render-from-state**: every mutation (`addTask`, `moveTask`, `deleteTask`, filter changes) updates `state` and calls `renderBoard()`, which rebuilds all columns via `renderCard()` and refreshes the summary strip. Card contents are never mutated directly outside `renderBoard()`. `renderBoard(focusSelector)` restores keyboard focus after a re-render — pass a selector when an action is keyboard-driven.
- **Event delegation**: click, keydown and drag/drop listeners are attached once to `#board` and dispatch on `data-action` / `data-id` attributes, so they survive re-renders.
- **XSS**: all user-supplied strings go through `escapeHtml()` before being interpolated into `innerHTML` templates; toasts use `textContent`.
- **Dates**: `YYYY-MM-DD` strings in local time (`toIsoDate`, `todayIso`); string comparison is used for overdue checks and the not-in-the-past validation. Seed tasks use `daysFromToday(n)` so overdue badges stay meaningful whatever the current date.
- **Add Task flow** (`handleSubmit`): validate with `validateTask` and show inline errors with `showFormErrors`, then optimistically `addTask` and close the `<dialog>`. Only then `await notifyNewTask` with the submit button in a "Sending…" state (also mirrored on the header button through `data-sending`). Errors show a warning toast and keep the card. `notifyNewTask` also treats a 200 response carrying `success: "false"` (e.g. FormSubmit not activated) as a failure.

---
name: web-ui-polish
description: Use when redesigning or polishing the visual design of this website.
---

# Web UI Polish

Apply this design system when redesigning or polishing the website. Keep all
existing content and behaviour (sections, form validation, links). Change how
it looks, not what it does.

## How to work

1. **Before:** screenshot the current page at 1440px and 375px (Playwright).
2. **Tokens:** put every value below into CSS custom properties on `:root`.
   Never hard-code a hex value, size or shadow in a component rule.
3. **Restyle** each section using only those tokens.
4. **Check:** run the accessibility rules at the end of this file and fix
   every failure.
5. **After:** screenshot at 1440px and 375px, and report what changed.

## 1. Colour palette

| Role | Token | Hex | Use |
|---|---|---|---|
| Primary | `--color-primary` | `#0e5a8a` | Primary buttons, links, active states, focus ring |
| Primary hover | `--color-primary-hover` | `#0a4870` | Primary button and link hover |
| Primary tint | `--color-primary-tint` | `#e6f1f8` | Hero background wash, icon chips, selected rows |
| Accent | `--color-accent` | `#c2410c` | Accent text: eyebrow labels, highlights, star ratings |
| Accent bright | `--color-accent-bright` | `#f97316` | Decorative shapes, badges with ink text only (never for text on white) |
| Ink (text) | `--color-ink` | `#0f172a` | Headings and body text; dark footer background |
| Muted text | `--color-muted` | `#475569` | Secondary text, captions, helper text |
| Border | `--color-border` | `#cbd5e1` | Card borders and dividers (decorative) |
| Input border | `--color-input-border` | `#64748b` | Form field borders (needs 3:1 against white) |
| Surface | `--color-surface` | `#ffffff` | Page and card background |
| Surface alt | `--color-surface-alt` | `#f5f7fa` | Alternating section background |
| Error | `--color-error` | `#b42318` | Error text and invalid field borders |
| Success | `--color-success` | `#067647` | Success message text |
| Focus on dark | `--color-focus-on-dark` | `#7cc4f0` | Focus ring inside dark sections (footer) |

Measured contrast:

| Pair | Ratio |
|---|---|
| White on primary | 7.4:1 |
| Primary on white | 7.4:1 |
| Accent on white | 5.2:1 |
| Ink on white | 17.9:1 |
| Muted on white | 7.6:1 |
| Muted on surface alt | 7.1:1 |
| Ink on accent bright | 6.4:1 |
| Error on white | 6.6:1 |
| Success on white | 5.7:1 |

Rules:
- One primary and one accent. Don't add new hues.
- Error and success colours are only for form feedback. Always pair them with
  text, never colour alone.
- Avoid purple-to-blue gradients and neon accents. A subtle hero background is
  a tint-to-white gradient (`--color-primary-tint` to `--color-surface`) or a
  faint dot or grid pattern in `--color-border`.

## 2. Typography

Google Fonts pairing, loaded with one `<link>` and `display=swap`:

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Manrope:wght@600;700;800&family=Source+Sans+3:wght@400;600&display=swap" rel="stylesheet">
```

- **Display (headings, buttons, numbers):** `--font-display: "Manrope", "Segoe UI", system-ui, sans-serif`
- **Body (paragraphs, labels, inputs):** `--font-body: "Source Sans 3", "Segoe UI", system-ui, sans-serif`

Type scale (desktop, then 375px mobile):

| Token | Desktop | Mobile | Font / weight | Line height | Use |
|---|---|---|---|---|---|
| `--text-display` | 56px | 36px | Manrope 800 | 1.1 | Hero headline (one per page) |
| `--text-h2` | 36px | 28px | Manrope 700 | 1.2 | Section headings |
| `--text-h3` | 22px | 20px | Manrope 700 | 1.3 | Card titles |
| `--text-lead` | 20px | 18px | Source Sans 3 400 | 1.5 | Hero subheading, section intros |
| `--text-body` | 17px | 16px | Source Sans 3 400 | 1.6 | Paragraphs, testimonial quotes |
| `--text-small` | 14px | 14px | Source Sans 3 600 | 1.4 | Labels, captions, helper and error text |
| `--text-eyebrow` | 13px | 13px | Manrope 700, uppercase, 0.08em tracking | 1.2 | Labels above section headings (accent colour) |

Rules:
- Headings use `text-wrap: balance`.
- Keep paragraph width at or under 65ch.
- Use `clamp()` to scale between the mobile and desktop sizes.
- Never set text smaller than 14px.

## 3. Spacing (8px scale)

| Token | Value |
|---|---|
| `--space-1` | 8px |
| `--space-2` | 16px |
| `--space-3` | 24px |
| `--space-4` | 32px |
| `--space-5` | 48px |
| `--space-6` | 64px |
| `--space-7` | 96px |

Use only these values for margin, padding and gap. The only exceptions are a
4px gap inside very small elements (icon to text) and 1–2px borders.

| What | Desktop | Mobile |
|---|---|---|
| Section vertical padding | `--space-7` | `--space-6` |
| Gap between cards | `--space-3` | `--space-3` |
| Card padding | `--space-4` | `--space-3` |
| Gap between form fields | `--space-3` | `--space-3` |
| Page side gutter | `--space-4` | `--space-2` (16px) |

- **Max content width:** 1200px, centred.
- **Lay out with gap:** flex or grid with `gap`, rather than margins on each child.

## 4. Corner radius and shadows

| Token | Value | Use |
|---|---|---|
| `--radius-sm` | 8px | Buttons, inputs, selects, badges |
| `--radius-md` | 16px | Cards, the form panel, tooltips |
| `--radius-full` | 999px | Avatars, pill badges, floating action buttons |

Shadows (soft and cool-tinted, never pure black):

| Token | Value | Use |
|---|---|---|
| `--shadow-sm` | `0 1px 2px rgba(15, 23, 42, 0.06), 0 1px 3px rgba(15, 23, 42, 0.08)` | Cards at rest, inputs on focus |
| `--shadow-md` | `0 4px 12px rgba(15, 23, 42, 0.08), 0 2px 4px rgba(15, 23, 42, 0.06)` | Card hover, the form panel |
| `--shadow-lg` | `0 12px 32px rgba(15, 23, 42, 0.12)` | Floating buttons, sticky header when scrolled |

Use one radius per element type across the whole site. Don't put a shadow on
every block: sections get a background change, and only cards, the form panel
and floating elements get shadows.

## 5. Components

Every interactive element is at least 44×44px. Transitions are 150–200ms ease
on colour, shadow and transform only.

### Buttons

| Variant | Default | Hover | Focus (keyboard) | Disabled |
|---|---|---|---|---|
| **Primary** | White text on `--color-primary`, Manrope 700 16px, padding `14px 24px` (min-height 48px), `--radius-sm` | `--color-primary-hover` background, `translateY(-1px)`, `--shadow-sm` | Focus ring (see section 7) | 50% opacity, `cursor: not-allowed`, no hover |
| **Secondary** | `--color-primary` text, transparent background, 2px `--color-primary` border | `--color-primary-tint` background | Focus ring | Same as primary |
| **Text link** | `--color-primary`, underline offset 3px | `--color-primary-hover`, underline thickens to 2px | Focus ring | — |

Rules:
- One primary button per section.
- Button labels say what happens: "Get a quote", not "Submit" or "Click here".

### Cards (services, testimonials)

- **Default:** `--color-surface` background, 1px `--color-border` border,
  `--radius-md`, `--shadow-sm`, padding `--space-4`.
- **Hover** (only if the card is clickable or decorative lift is wanted):
  `--shadow-md` and `translateY(-2px)`.
- **Focus-within:** focus ring on the card if it contains the only link.
- **Same height:** cards in a row match height (grid `align-items: stretch`)
  and their contents line up.
- **Testimonial card:**
  - a large opening quote mark in `--color-primary-tint` or `--color-accent`
    (decorative, `aria-hidden`)
  - the quote in `--text-body`, italic optional
  - then a row with a 48px initials avatar (`--radius-full`, white text on
    `--color-primary`, Manrope 700) followed by the name (Manrope 700) and the
    job title and company (`--text-small`, `--color-muted`)
  - star ratings, if used, in `--color-accent` with a visually-hidden
    "5 out of 5" label
- **Service card:** a 48px icon chip (`--color-primary-tint` background,
  `--color-primary` icon, `--radius-sm`), then the `--text-h3` title, then
  body text.

### Form fields

| State | Style |
|---|---|
| Default | `--color-surface` background, 1px `--color-input-border`, `--radius-sm`, padding `12px 16px`, min-height 48px, `--text-body`, `--color-ink` text |
| Placeholder | `--color-muted` (never a replacement for the label) |
| Hover | Border `--color-ink` |
| Focus | Border `--color-primary`, plus the focus ring, plus `--shadow-sm` |
| Invalid | 2px `--color-error` border, an error message below in `--color-error` `--text-small` with an icon, and `aria-invalid="true"` linked by `aria-describedby` |
| Disabled | `--color-surface-alt` background, `--color-muted` text |

Rules:
- **Labels:** always visible, above the field, `--text-small` 600 in
  `--color-ink`. Mark required fields with text ("Required") or an asterisk
  explained once.
- **Form panel:** `--color-surface`, `--radius-md`, `--shadow-md`, padding
  `--space-5` on desktop and `--space-3` on mobile.
- **Success message:** `--color-success` text with a check icon, announced
  with `role="status"`.

## 6. Responsive rules

Design mobile-first. The three reference widths are **375px**, **768px** and
**1440px**.

| Width | Layout |
|---|---|
| **375px** (≤ 767px) | One column throughout. Side gutter 16px. Hero headline 36px, buttons full width and stacked with a `--space-2` gap. Cards stack (testimonials one per row). Form fields full width. Nav collapses to a 44px menu button, or to a short row of links that wraps. Section padding `--space-6`. |
| **768px** (768–1199px) | Two columns for services and testimonials (the third card spans or wraps cleanly; no orphan stretched across). Hero text left-aligned, max 640px. Form panel max 640px, centred. Side gutter `--space-4`. |
| **1440px** (≥ 1200px) | Content max-width 1200px, centred. Three columns for services and testimonials. Hero can be split: text on the left, a visual or form preview on the right. Section padding `--space-7`. |

Rules at every width:
- No horizontal page scroll; check with `scrollWidth === innerWidth`.
- Images and media use `max-width: 100%`.
- Any flex or grid child holding text gets `min-width: 0`.
- Fixed or floating elements (e.g. a chat button) stay inside the viewport,
  clear of the form's submit button, and respect
  `env(safe-area-inset-bottom)`.

## 7. Accessibility rules (must pass before finishing)

1. **Contrast:**
   - Body text and labels: at least **4.5:1** against their background.
   - Large text (24px+, or 19px+ bold) and UI component boundaries (input
     borders, focus rings, icons that carry meaning): at least **3:1**.
   - Use only the colour pairs listed in section 1. If you add a pair, measure
     it and add it to that table.
2. **Visible focus ring** on every interactive element, using `:focus-visible`:
   ```css
   :focus-visible { outline: 3px solid var(--color-primary); outline-offset: 3px; }
   .dark-section :focus-visible { outline-color: var(--color-focus-on-dark); }
   ```
   Never `outline: none` without a replacement. The ring must reach 3:1
   against the surface around it.
3. **Tap targets** of at least **44×44px** for buttons, links in nav, form
   controls, icon buttons and floating buttons. Pad small inline links rather
   than leaving them tiny.
4. **Keyboard:** everything works with Tab, Shift+Tab, Enter and Space, in a
   logical order. There's no keyboard trap.
5. **Semantics:**
   - One `<h1>`, with headings in order.
   - Landmarks: `header`, `nav`, `main`, `footer`.
   - Every input has a `<label for>`.
   - Icon-only buttons have an `aria-label`.
   - Decorative icons have `aria-hidden="true"`.
6. **Motion:** wrap hover lifts, pulses and scroll animations in
   `@media (prefers-reduced-motion: no-preference)`. Nothing flashes.
7. **Verify:** at 375px and 1440px, measure contrast of any new colour pairs,
   tab through the page to confirm the focus ring is visible, and measure
   button and input sizes with `getBoundingClientRect()`. Report
   pass or fail for rules 1–6.

## Output

When finished, report:
1. What changed (palette, type, layout, components), in a short list
2. Before and after screenshots at 1440px and 375px
3. Accessibility results (rules 1–6, Pass / Fix)

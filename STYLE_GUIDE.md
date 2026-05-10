# Vietnam 2026 — Site Style Guide

A reference for every design decision in `index.html`: colours, typography, spacing, components, and patterns. Use this if you want to extend the site, restyle a section, or rebuild it in a different framework.

---

## Colour Palette

All colours are defined as CSS custom properties on `:root` and used via `var()` throughout. Never use raw hex values directly in component styles — always reference a variable.

| Variable | Hex | Usage |
|----------|-----|-------|
| `--cream` | `#F7F3ED` | Page background, hero text |
| `--deep` | `#1A1612` | Hero background, day card headers, tab active state, body text |
| `--gold` | `#C9973A` | Primary accent — headings, arrows, dots, highlights |
| `--gold-light` | `#F0D89A` | Reserved for subtle gold tints (currently unused, available) |
| `--red` | `#B84040` | Warning text, warn-pill, warn-box border |
| `--green` | `#3A7A5A` | Success/cost, map links, checkboxes |
| `--green-light` | `#EBF4EE` | Cost pill backgrounds, success tints |
| `--muted` | `#7A7268` | Secondary text, labels, metadata |
| `--border` | `rgba(26,22,18,0.12)` | All card and component borders |
| `--card-bg` | `#FFFDF9` | Card and todo item backgrounds |
| `--amber-bg` | `#FFF8E8` | Hotel rows, note boxes, todo tags |
| `--red-bg` | `#FFF0F0` | Warning boxes, warn tags, warn pills |

### Semantic colour system

- **Gold** = primary brand, wayfinding, emphasis
- **Green** = positive status, bookable, costs, links
- **Red** = warnings, urgency, not-yet-booked
- **Amber** = informational callouts, hotels, notes
- **Deep** = structural chrome (hero, card headers, active states)
- **Cream** = breathing room, backgrounds

---

## Typography

Two typefaces loaded from Google Fonts:

```html
<link href="https://fonts.googleapis.com/css2?family=Playfair+Display:ital,wght@0,400;0,600;1,400&family=DM+Sans:wght@300;400;500&display=swap" rel="stylesheet">
```

### Playfair Display (serif)
Used for: hero heading, destination names, section titles, flight codes in tables, budget values, stat numbers.

The italic variant (`font-style: italic`) is used selectively for the "2026" in the hero H1 and nowhere else — it's a one-off editorial accent, not a general pattern.

### DM Sans (sans-serif)
Used for: everything else — body copy, labels, tabs, activity lists, metadata, table content.

### Type scale

| Element | Font | Size | Weight | Notes |
|---------|------|------|--------|-------|
| Hero H1 | Playfair Display | `clamp(42px, 7vw, 72px)` | 400 | Fluid, responsive |
| Hero H1 `em` | Playfair Display | inherited | 400 italic | Gold colour |
| Hero label | DM Sans | 11px | 500 | Uppercase, 0.2em tracking |
| Hero subtitle | DM Sans | 15px | 300 | 55% opacity cream |
| Hero stat value | Playfair Display | 28px | 400 | Gold |
| Hero stat label | DM Sans | 11px | 400 | Uppercase, 45% opacity |
| Section title | Playfair Display | 22px | 400 | |
| Section meta | DM Sans | 12px | 400 | Uppercase, muted |
| Destination name | Playfair Display | 20px | 600 | |
| Destination number | Playfair Display | 36px | 400 | Gold, 50% opacity |
| Day card header | DM Sans | 12px | 500 | Uppercase feel via letter-spacing |
| Activity list items | DM Sans | 13.5px | 400 | |
| Tab buttons | DM Sans | 12px | 500 | 0.03em tracking |
| Table headers | DM Sans | 11px | 500 | Uppercase, muted |
| Table body | DM Sans | 13px | 400 | |
| Note/warn boxes | DM Sans | 13px | 400 | |
| Pills and tags | DM Sans | 10–11px | 500 | |
| Map links | DM Sans | 11px | 400 | |
| Body default | DM Sans | 15px | 400 | line-height 1.6 |

---

## Spacing System

No formal spacing scale — spacing is set contextually per component. Common values used:

| Usage | Value |
|-------|-------|
| Section gap (margin-top) | 48px |
| Main content padding | 40px horizontal, 40px top, 80px bottom |
| Hero padding | 60px top, 40px sides, 48px bottom |
| Destination block gap | 40px margin-bottom |
| Day card gap | 12px |
| Todo item gap | 6px |
| Contact card gap | 10px |
| Budget card gap | 12px |
| Card internal padding | 14–16px |
| Border radius — cards | 10px |
| Border radius — pills | 10–12px |
| Border radius — tags | 10px |
| Border radius — note boxes | 0 6px 6px 0 (left-border style) |
| Border radius — buttons | 20px (pill shape) |

---

## Layout

### Overall structure

```
<body>
  .hero                     ← full-width dark header
  .flight-banner            ← full-width dark sub-header strip
  .main                     ← max-width 900px, centred, padded
    .tab-nav                ← sticky navigation
    #panel-*                ← content panels (one visible at a time)
```

### Max width

The content column is capped at `900px` and centred with `margin: 0 auto`. Both the hero and flight banner have their own inner wrapper (`.hero-inner`, `.flight-banner-inner`) that also apply this max-width, keeping text aligned to the same column as the main content below.

### CSS Grid usage

Three grid layouts used:

1. **Destination header** — `grid-template-columns: 48px 1fr auto` — number / name+dates / nights pill
2. **Timeline** — `grid-template-columns: 80px 24px 1fr` — dates / dot+track / content
3. **Budget grid** — `grid-template-columns: repeat(auto-fit, minmax(200px, 1fr))` — responsive cards
4. **Contacts grid** — `grid-template-columns: repeat(auto-fit, minmax(240px, 1fr))` — responsive cards

### Flexbox usage

Used for: hero meta stats row, flight banner items, day card headers (space-between), activity list items, hotel rows, todo items, tab nav, timeline dot column.

---

## Components

### Hero

Dark (`--deep`) full-width section with a subtle 45° diagonal stripe pattern via a `::before` pseudo-element using `repeating-linear-gradient`. The stripe uses gold at 4% opacity — barely visible, adds texture without distraction.

```css
.hero::before {
  background: repeating-linear-gradient(
    45deg,
    transparent, transparent 40px,
    rgba(201,151,58,0.04) 40px,
    rgba(201,151,58,0.04) 41px
  );
}
```

The hero contains: a gold uppercase label, the large serif H1 (with italic gold "2026"), a muted subtitle, and a stats row of 4 key facts separated by 1px vertical dividers.

---

### Flight Banner

A horizontal scrolling strip (`overflow-x: auto`, `white-space: nowrap`) sitting immediately below the hero, still on the dark background, separated by a subtle gold-tinted top border. Each flight item is separated by a gold-tinted right border. Used for the 4 confirmed Qatar flights.

---

### Tab Navigation

Sticky (`position: sticky; top: 0`) with a cream background and a bottom border, sitting above the content panels. Tab buttons are pill-shaped (border-radius 20px). Three states:

- **Default:** transparent background, muted text, light border
- **Hover:** gold border and text
- **Active:** deep background, cream text

Only one panel (`#panel-*`) is visible at a time. Inactive panels have `display: none`. Switching is handled by a small JavaScript function `showTab(name)`.

---

### Destination Block

Each destination follows this structure:

```
.destination-block
  .dest-header (3-column grid)
    .dest-num          ← muted gold serif number (01, 02…)
    div
      .dest-name       ← bold serif city name
      .dest-dates      ← muted date string
      .weather-pill    ← coloured status pill
    .dest-nights       ← dark pill, e.g. "2 nights"
  .hotel-row           ← amber callout with emoji icon
  .note-box / .warn-box  ← optional callouts
  .days-grid           ← column of day cards
```

---

### Day Card

```
.day-card
  .day-card-header     ← dark background, white text, date left / subtitle right
  .day-card-body
    ul.activity-list
      li               ← gold arrow pseudo-element, text, optional .map-link
```

The gold `→` arrow on each list item is a CSS `::before` pseudo-element (`content: '→'`), not a character in the HTML. Each item has a faint bottom border except the last child.

---

### Map Link

Small outlined pill button (`.map-link`) that sits inline within an activity list item. Flush right via `flex-shrink: 0`. Green border and text, fills solid green on hover with white text.

---

### Callout Boxes

Two variants, both using a left-border style:

**Note box** (amber) — informational, tips, logistics context
```css
background: var(--amber-bg);
border-left: 3px solid var(--gold);
border-radius: 0 6px 6px 0;
color: #5A4800;
```

**Warn box** (red) — weather warnings, critical timing alerts
```css
background: var(--red-bg);
border-left: 3px solid var(--red);
border-radius: 0 6px 6px 0;
color: var(--red);
```

---

### Hotel Row

An amber card (`--amber-bg`) with a gold-tinted border, used once per destination to show accommodation details. Flex layout: emoji icon left, name + detail text right.

---

### Weather Pills

Three variants applied to `.weather-pill`:

| Class | Background | Text | Meaning |
|-------|-----------|------|---------|
| `.weather-ok` | `--green-light` | `--green` | Good weather ✓ |
| `.weather-ok-ish` | `--amber-bg` | `#7A5A00` | Mixed / warm caution |
| `.weather-warn` | `--red-bg` | `--red` | Bad weather ⚠ |

---

### Status Pills (inline)

| Class | Background | Text | Usage |
|-------|-----------|------|-------|
| `.cost-pill` | `--green-light` | `--green` | Estimated flight/activity costs |
| `.warn-pill` | `--red-bg` | `--red` | "Book Oct", "Urgent" status labels |

---

### To-Do Items

Each `.todo-item` is a flex row: checkbox square / text / tag pill. Clicking toggles the `.done` class via JavaScript, which:
- Sets the item to 45% opacity
- Strikes through the text
- Fills the checkbox green with a `✓` character

Tag variants:

| Class | Background | Text | Meaning |
|-------|-----------|------|---------|
| `.todo-tag` (default) | `--amber-bg` | `#7A5A00` | By August / By October / Pre-trip |
| `.todo-tag.urgent` | `--red-bg` | `--red` | Do Now |

---

### Tables

Three table classes (`.flights-table`, `.transfers-table`, `.points-table`) share the same styling: no outer border, `border-collapse: collapse`, subtle row dividers, uppercase muted column headers with 0.1em tracking, and a gold-tint hover state on rows. Tables are wrapped in a card container (white background, border, border-radius) rather than being styled directly.

---

### Timeline (Overview panel)

A vertical timeline built with CSS Grid. Each row (`.tl-item`) has three columns:

1. **Dates** — right-aligned, small muted text
2. **Visual track** — a 10px gold dot (`border-radius: 50%`) sitting atop a 1px vertical line (`--border` colour) that stretches to the next item
3. **Content** — place name (medium weight) and info line (small, muted)

Departure/arrival dots use the gold colour directly; destination dots use the default `--gold` variable.

---

### Budget Cards & Total

Budget cards (`.budget-card`) use `repeat(auto-fit, minmax(200px, 1fr))` to reflow into however many columns fit. The total row (`.budget-total`) is a full-width dark card with the total value in large gold Playfair Display text, right-aligned.

---

### Contact Cards

A responsive grid (`.contacts-grid`) of small cards (`minmax(240px, 1fr)`). Each has a bold name, a green anchor link, and a muted phone/detail line. Links are `display: block` so the entire line is clickable.

---

## Responsive Behaviour

One breakpoint at `600px`:

```css
@media (max-width: 600px) {
  .hero { padding: 40px 20px 36px; }
  .main { padding: 28px 20px 60px; }
  .dest-header { grid-template-columns: 36px 1fr; }
  .dest-nights { display: none; }
}
```

On mobile:
- Hero and main padding reduce from 40px to 20px horizontal
- Destination header drops to a 2-column grid (number + content), hiding the nights pill
- Tab nav wraps naturally (`flex-wrap: wrap`)
- Flight banner scrolls horizontally (`overflow-x: auto`)
- Budget and contact grids reflow to a single column via `auto-fit`

---

## JavaScript

Two small functions — no libraries or frameworks.

### `showTab(name)`
Hides all `.panel` elements and deactivates all `.tab-btn` elements, then shows `#panel-{name}` and marks the clicked button as `.active`. Called via inline `onclick` on each tab button.

```javascript
function showTab(name) {
  document.querySelectorAll('.panel').forEach(p => p.classList.remove('active'));
  document.querySelectorAll('.tab-btn').forEach(b => b.classList.remove('active'));
  document.getElementById('panel-' + name).classList.add('active');
  event.target.classList.add('active');
}
```

### `toggleTodo(el)`
Toggles the `.done` class on a todo item and sets the checkbox text to `✓` or empty accordingly.

```javascript
function toggleTodo(el) {
  el.classList.toggle('done');
  const check = el.querySelector('.todo-check');
  check.textContent = el.classList.contains('done') ? '✓' : '';
}
```

> ⚠ Todo state is not persisted — refreshing the page resets all checkboxes. To persist state, `localStorage` would need to be added.

---

## Extending the Site

### Adding a new destination block

Copy an existing `.destination-block` div. Update:
- `.dest-num` — next sequential number
- `.dest-name`, `.dest-dates`, `.dest-nights`
- `.weather-pill` class — use `weather-ok`, `weather-ok-ish`, or `weather-warn`
- `.hotel-row` content
- `.day-card` entries inside `.days-grid`

### Adding a new tab panel

1. Add a button to `.tab-nav`: `<button class="tab-btn" onclick="showTab('myname')">Label</button>`
2. Add a panel: `<div id="panel-myname" class="panel">…</div>`

### Changing the accent colour

Replace `--gold: #C9973A` in `:root`. All gold elements will update automatically — hero H1 em, destination numbers, arrows, dots, dividers, flight codes, note box borders, stat values.

### Changing the background

Replace `--cream: #F7F3ED` and `--card-bg: #FFFDF9`. If switching to a dark background, also update `--border`, `--deep` for text colour, and the tab nav sticky background.

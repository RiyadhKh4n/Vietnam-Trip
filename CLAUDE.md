# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

A single-page static itinerary site for a Vietnam trip (Nov 5–29, 2026). The entire site is one self-contained file: `index.html` (~1500 lines) with inline `<style>` and `<script>` — no build step, no dependencies, no package manager. Open the file directly in a browser to view.

Two Google Fonts (Playfair Display, DM Sans) are the only external resources.

## Architecture

The page is structured around a tab-panel pattern driven by two tiny inline JS functions at the bottom of `index.html`:

- `showTab(name)` — hides all `.panel` elements, shows `#panel-{name}`, toggles `.active` on tab buttons. Called from inline `onclick` handlers on `.tab-btn` elements.
- `toggleTodo(el)` — toggles `.done` on a `.todo-item` and swaps its checkbox character. **State is not persisted** (no localStorage); a refresh resets all checkboxes.

Six panels exist: `itinerary`, `overview`, `logistics`, `todo`, `budget`, `contacts`. Adding a new panel requires both a `<button class="tab-btn" onclick="showTab('x')">` in `.tab-nav` and a matching `<div id="panel-x" class="panel">`.

Layout: full-width `.hero` + `.flight-banner` (dark, `--deep`) sit above a `.main` content column capped at 900px. One responsive breakpoint at 600px.

## Styling rules

`STYLE_GUIDE.md` is the source of truth for design decisions. Key rules when editing styles:

- **Never hard-code colours.** All colours are CSS custom properties on `:root` (`--cream`, `--deep`, `--gold`, `--green`, `--red`, `--amber-bg`, `--red-bg`, etc.) — always reference via `var()`.
- **Semantic colour usage:** gold = brand/wayfinding, green = positive/cost/links, red = warnings, amber = informational callouts, deep = chrome.
- **Typography is two-font:** Playfair Display (serif) for headings/numbers/flight codes/budget values; DM Sans for everything else. The italic Playfair variant is used **only** for the "2026" in the hero H1 — don't reuse it elsewhere.
- **Weather pills** have three preset variants: `.weather-ok`, `.weather-ok-ish`, `.weather-warn`. **Status pills:** `.cost-pill` (green) and `.warn-pill` (red).
- **Callout boxes** use the left-border style (`border-radius: 0 6px 6px 0`): `.note-box` (amber/gold) for info, `.warn-box` (red) for alerts.
- The gold `→` on activity list items is a `::before` pseudo-element, not a character in the HTML — don't add arrows manually.

## Adding a destination block

Copy an existing `.destination-block` and update: `.dest-num` (next sequential), `.dest-name`, `.dest-dates`, `.dest-nights`, the `.weather-pill` class, `.hotel-row` content, and the `.day-card` entries inside `.days-grid`. See `STYLE_GUIDE.md` for the full structure.

# AGENT.md

This repository builds MARP slides via `Taskfile.yml` using `slides/deck.md` and `themes/deck_theme.css`. The deck always contains a **frontpage** (title slide) and a **closing** slide; all user-provided slides are inserted between them.

Use these rules when turning a user prompt into `slides/deck.md` and (if needed) extending `themes/deck_theme.css`.

## Build Context
- Build runs via Taskfile tasks (dockerized Marp CLI).
- The theme is `deck_theme` and expects HTML enabled.
- Slide size is 16:9.

## Deck Skeleton (always keep)
Keep the first and last slide untouched:
- **Frontpage**: `<!-- _class: title -->` with `# Title`, optional `.subtitle`, `.date`.
- **Closing**: `<!-- _class: closing -->` (can be empty).

## Input Format
Each user slide arrives as:
- `## Title` (slide title)
- `## Structure` (layout description)
- `## Content` (content blocks)  
Or a single `## Section` (section divider title).

## Core Rules
- Insert new slides **between** the frontpage and closing slide.
- Use **existing CSS classes first**; only extend CSS if no existing pattern matches.
- Prefer semantic Markdown (`##` titles, lists, tables) plus minimal HTML for layout.
- Use `<!-- _class: content -->` for normal slides. Add modifiers as needed (see below).
- Add `no-top-accent` to slides with heavy boxes/tiles to avoid a double-strong visual top line.
- Keep slide density moderate: avoid more than ~6 bullet lines per column.

## Class & Layout Mapping
Use these mappings for common structures (match keywords from `## Structure`):

### Section
- Use: `<!-- _class: section -->` and `# Section Title`.

### Two Columns
- Use:
  - `<!-- _class: content -->`
  - `<div class="columns two-col"> ... </div>`
- Variants:
  - Soft panels: add `.duo` to `columns` for subtle rail styling.
  - Split panels: use `.split-panels` + `.panel` for strong two-card layout.

### Highlight/Quote Box
- Use `.highlight-box` with `.highlight-title` and `.highlight-quote`.
- Add `no-top-accent`.

### Ranked List
- Use `<ol class="ranked-list">...</ol>`.
- Add `hero hero-ranked` if it should be vertically centered.

### Statement + Bullets
- Use `<div class="statement"><p>...</p></div>` then a `<ul>`.
- Add `bottom-bar` if the slide expects a soft bottom rule.

### Process / Steps
- Use `<div class="process">` with `.process-step`.
- For horizontal progression: add `process-flow` class to the slide and wrap with `flow-cards`.

### Cards / Tiles / Grids
- Tools cards: `<!-- _class: content tools-cards no-top-accent -->` + `.tools-grid`.
- Tiles: `.grid-tiles` + `.tile` or `.labs-grid` + `.lab-tile` (if slide labeled “focus areas”).

### Capability / Diagram / Lifecycle
- Use `<!-- _class: content capability-balanced no-top-accent -->` with `.capability-balanced-layout`.
- Use `<!-- _class: content lifecycle no-top-accent -->` for a left rail + right visual.

### Tables
- Prefer existing card grids first. If a table is required, use a simple HTML table and add `no-top-accent`.
- Only add new table styles if the grid card style cannot represent the content.

## Additional Structure Mappings

### Full-width Title + Statements + Closing Line
- Use: `<!-- _class: content hero -->`
- Structure:
  - `## Title`
  - `<div class="impact-list">` for 2–4 punchy statements
  - `<p class="closing-line">` for the final line
- Add `no-top-accent` if the slide feels visually heavy.

### Two Columns: Facts vs Implications
- Use: `columns two-col duo` for softer rails.
- Add `h3` headings in each column.

### Two Columns: Left List + Right Quote
- Use: `columns two-col highlight-layout`
- Left: `ul`
- Right: `.highlight-box` with `.highlight-title` and `.highlight-quote`
- Add `no-top-accent`.

### Statement + List with Emphasis
- Use: `.statement` followed by `ul` or `ol`.
- For a softer emphasis bar, add `bottom-bar` on the slide.

### Ranked Vertical List
- Use: `hero hero-ranked` if it should be vertically centered, otherwise plain `content`.
- Use `ol.ranked-list`.

### Comparison Split (IS vs IS NOT)
- Use: `columns compare split-panels` with `.panel` children.
- Add `no-top-accent`.

### Matrix / 2x2 / Side-by-side Concepts
- Use: `columns two-col duo`
- Represent each quadrant as a column with an `h3` and short `p` lines.

### Diagram-centric (Center + Notes)
- Use: `content capability-balanced no-top-accent` if it fits.
- Otherwise create a simple two-column grid with a boxed center element and side notes.

### Process Diagram (3–5 Steps)
- Use: `process-flow` class with `.process` and `.process-step`.
- Add `no-top-accent` if cards dominate the slide.

### Tooling Proposal / Options Grid
- Use: `content tools-cards no-top-accent` with `.tools-grid` and `.tool-card`.

### Focus Areas / Tiles
- Use: `content labs-focus no-top-accent` with `.labs-grid` and `.lab-tile`.

### Investment / Framework + Validation
- Use: `content investment no-top-accent` with `.investment-layout`.

## CSS Extension Rules
If a structure has **no** matching class:
- Add a **minimal, reusable** class.
- Prefer generic utilities (`.grid-3`, `.card`, `.pill`) over slide-specific names.
- Keep borders light, use `--so-radius` and `--so-shadow` for consistency.
- Avoid heavy contrast: use `--so-line`, `--so-muted`, `--so-accent-soft`.

## Slide Metadata (required)
At top of `slides/deck.md`:
```
---
marp: true
title: <presentation title>
theme: deck_theme
paginate: true
html: true
size: 16:9
---
```

## Assets
- Use relative paths from `slides/`, e.g. `../assets/...`.
- Do not embed new images unless explicitly provided by the user.

## Output Expectation
- The deck should remain clean, readable, and consistent with the current theme.
- If you add CSS, keep it concise and avoid duplicating existing patterns.

## Source Artifacts
- `definition/deck-definition.md` captures the input content definition.
- `layout/slide-layout.md` captures the planned slide layout derived from the definition.

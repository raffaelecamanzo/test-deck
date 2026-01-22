 # Test Deck

This repository contains a MARP slide deck and theme, along with a Taskfile-based build workflow.

## Contents
- `slides/deck.md`: The MARP deck source.
- `themes/deck_theme.css`: The custom theme (`@theme deck_theme`).
- `assets/`: Local images used by the deck.
- `definition/deck-definition.md`: Source content definition.
- `layout/slide-layout.md`: Slide layout plan derived from the definition.
- `output/`: Generated PDFs and PPTX files.

## Build & Preview
All tasks run Marp CLI via Docker.

```
task preview   # live preview at http://localhost:8080
task pdf       # export versioned PDF to output/
task pptx      # export versioned PPTX to output/
task artifacts # list generated artifacts
task clean     # remove output/
```

## Deck Metadata
The deck front matter in `slides/deck.md` must include:

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

## Notes
- Theme name in `slides/deck.md` must match the `@theme` declaration in `themes/deck_theme.css`.
- Local assets should be referenced with paths relative to `slides/` (e.g. `../assets/...`).

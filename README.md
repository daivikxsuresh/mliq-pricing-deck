# MLiQ — Data-driven pricing (live deck)

Built Aug 16 2026 off **"MLiQ Pricing Model Slides v3"** (Google Slides). Same lineage as the
Jun 9 capstone deck — reuses the `<deck-stage>` engine, but dark to match the v3 slides.

## Run it

Just open `index.html` — works straight off the filesystem, no server, no build.

- `←` `→` / `PgUp` `PgDn` / `Space` — navigate
- number keys — jump to a slide
- `R` — reset to slide 1
- thumbnail rail on the left: click to jump, drag to reorder, right-click to skip/duplicate/delete

## Print / PDF

`Cmd+P` → Save as PDF gives one page per slide, no extra setup. `MLiQ_Deck.pdf` in this folder
is a pre-rendered copy (9 pages, 1.1MB).

Regenerate:

```
"/Applications/Google Chrome.app/Contents/MacOS/Google Chrome" --headless=new \
  --no-pdf-header-footer --virtual-time-budget=8000 \
  --print-to-pdf=MLiQ_Deck.pdf file://$PWD/index.html
```

## Editing

Every slide is static HTML inside `<deck-stage>` — click into the markup and retype. No JS
templating, no build step.

- Slide 9 has three **`EDIT ME`** placeholders (the open questions / the ask). Those are the only
  intentionally-unfinished strings in the deck.
- Counters: put the target in `data-count` with optional `data-prefix` / `data-suffix` /
  `data-decimals`. The **authored text is what ships** — the counter animates up to it and then
  restores the exact string, so the printed and reduced-motion versions are always correct.
- Bars: set `--w` inline (e.g. `style="--w:59.9%"`). Full width is the base state; the sweep only
  animates `transform`, never `width`.

## Design notes

Follows the portable motion foundation: `transform`/`opacity` only, `prefers-reduced-motion`
gates every animation, nothing loops decoratively. Palette is contrast-checked against the
near-black ground — body text 16.4:1, dim text 7.5:1, flame accent 7.6:1, signal blue 7.1:1,
white on the blue fill 6.6:1.

Two gotchas worth remembering if you extend this:

- **Never make a table cell `display:flex`** when it holds a sentence with inline `<strong>`.
  Each inline element becomes its own flex item and the sentence shreds into columns.
- **Never animate a bar by resetting inline `width`.** If the tab isn't painting, it stays at 0
  and prints empty. Animate `scaleX` off the true width instead.

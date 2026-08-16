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

The original MLiQ warm-paper theme — cream `#FAF6EE` / gold `#A57C2E`, Cormorant Garamond +
JetBrains Mono. Emphasis comes from the **inverted ink panel**, not a colour fill.

Follows the portable motion foundation: `transform`/`opacity` only, `prefers-reduced-motion`
gates every animation. Two motion layers:

1. **Entrance** — staggered rise, rules drawing left-to-right, bars sweeping, numbers counting.
2. **Ambient** — while a slide is up, the warm radial light breathes on a 17s cycle and the
   active footer tick pulses. Both are *light only* — nothing ever moves under the text, so a
   slide can sit on screen for a minute and still feel alive without being distracting.

All 351 text elements pass WCAG AA, verified in-browser rather than by eye (lowest ratio 3.06
against a 3.0 large-text floor). Three gotchas worth remembering if you extend this:

- **Check gold against the deepest surface it lands on**, not the page ground. `--gold-text`
  reads 5.4:1 on open paper but only 4.1:1 on `--paper-3`, so it's tuned to pass on the latter.
- **Inverted panels need explicit `em`/`strong` colours.** Both default to `--ink`, which is
  invisible on the ink card — one italic vanished entirely before this was caught.
- **Never animate a bar by resetting inline `width`,** and never make a table cell `display:flex`
  when it holds a sentence with inline `<strong>` — the first prints empty bars, the second
  shreds the sentence into columns.

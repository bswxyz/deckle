# Deckle — mould-made notebooks & fine paper goods

A design-showcase website template for **Parable**: the storefront of a fictional
small paper mill that has been pulling cotton-rag sheets on the same brass mould
since 1949. Talks gsm, deckle edges and ink feathering; encourages the analog.

**Live:** https://bswxyz.github.io/deckle/ · **Build notes:** [`/guide/`](https://bswxyz.github.io/deckle/guide/)

## The concept

The whole site behaves like one sheet of paper.

- **Re-rule the page.** A radio group swaps the site-wide substrate between
  blank / dot grid / ruled / graph. The swap is pure CSS — `body:has(input:checked)`
  rewrites two custom properties, and the fixed background, the hero notebook and
  the doodle pad all inherit them. Works without JavaScript; JS only remembers your
  choice. Ruled mode summons a red margin line, as it should.
- **A nib that answers your hand.** The scratch-pad canvas uses pointer speed as a
  pressure proxy: slow strokes run wet (wide), fast strokes go lean. Strokes are
  stored as points, so the drawing survives resizes and redraws in the right ink
  when the theme flips. A sample flourish writes itself when the section scrolls in.

## Design system

| Token | Light (default) | Dark |
| --- | --- | --- |
| `--bg` | kraft `#ece3d1` | ink-blue `#14171f` |
| `--ink` | ink-blue `#14171f` | kraft `#ece3d1` |
| `--hl` | highlighter `#f2c14e` (both) | — |
| `--blue` | second ink `#2b3a67` | `#9db1e3` |
| `--margin` | ruled margin `#b8563f` | `#cd7a63` |

- **Type:** Fraunces (display) · Karla (body) · Space Mono (specs, receipts, labels)
- **Signature ease:** `--ease: cubic-bezier(.3, .02, .18, 1.04)` — *nib-lift*: a slow
  press into the sheet, a light flick as the pen leaves.
- The highlighter yellow never sets text (it fails contrast on kraft); it appears only
  as swipes behind display italics and as button fills under near-black ink.
- Deckle edges are hand-jittered `clip-path` polygons — the notebook fore-edge and the
  till receipt's torn tail.

## Stack

- [Astro 5](https://astro.build), zero client framework — content lives as arrays in
  page frontmatter and renders to static HTML.
- One inline vanilla script: theme toggle, reveals, counters, the demo till and the
  doodle pad. No dependencies, no external images — all art is inline SVG/CSS/canvas.

## Run locally

```sh
npm install
npm run dev        # http://localhost:4321/deckle/
npm run build      # emits ./docs for GitHub Pages
```

## Structure

```
src/
├── pages/
│   ├── index.astro          # the site (data in frontmatter, inline script at the end)
│   └── guide.astro          # "How Deckle was built" → /guide/
├── components/
│   ├── RangeArt.astro       # line-drawn product art (6 kinds)
│   └── MouldDiagram.astro   # the mould & deckle, labelled
└── styles/
    └── global.css           # tokens for both themes at the top, then everything
public/.nojekyll             # keeps Pages from running Jekyll on /docs
astro.config.mjs             # site + base '/deckle' + outDir './docs'
```

## Demo vs. real

This is a front-end showcase. Specifically:

- **The till is a demo.** It validates, totals (with the £3-under-£30 postage rule),
  and confirms in place — nothing is sent, stored or charged.
- The mill, makers, prices, batch numbers and the Thursday post van are fiction.
- A real shop would need a commerce backend (cart, payments, inventory), real product
  photography, and an order-notification service.
- Doodles on the scratch pad never leave the page — there is no analytics or storage.

## Accessibility notes

- Light/dark themes with a persistent toggle (`localStorage: deckle-theme`), skip link,
  `:focus-visible` outlines, real form semantics (fieldset/legend radios).
- Full `prefers-reduced-motion` support: reveals arrive finished, the hero stands
  still, the sample flourish renders as a single static frame, and the nib draws at a
  fixed width. The paper toggle still swaps — a swap isn't motion.
- Content is never hidden without JavaScript: reveals are gated behind a `.js` class
  and the ruling swap is pure CSS.

## License

[MIT](./LICENSE) © 2026 Parable

# Design

<!-- impeccable:design-schema 1 -->

Recorded from the built home board (`index.html`), not from intention. The four
games each carry their own in-canvas art direction; this file governs the site
shell — today, the picker.

## The world

The picker is a **printed game board**. The whole viewport is the board: four
saturated ink *lands*, one per game, divided only by a printed keyline, with a
die-cut plaque and a paper playing piece on each. It deliberately refuses the
category default — a page of equal rounded cards carrying a thumbnail, a label
and a Play button on a friendly gradient.

Colour strategy is **drenched**: the surface *is* the four game worlds. Nothing
is a neutral ground except the ink that prints the seams.

## Tokens

```
--ink    #161020   board ink: seams, frame, plaque grounds, medallion
--paper  #FFF6E4   warm paper: type on ink, keylines, playing pieces
--key    4px       printed keyline between lands (6px at >=680px)
--inset  10px      how far a plaque sits in from its corner (18px at >=680px)
```

Each land carries two of its own inks, lifted from the world it opens:

| Land | `--land` (ground) | `--plaque` (label) |
|---|---|---|
| Starfall Meadow | `#2E8FC9` | `#0F5183` |
| Owlet Grove | `#3AA36E` | `#156045` |
| Beacon Bridge | `#6B4A72` | `#3E2758` |
| Foxglove Trail | `#1D4B3D` | `#0F3A30` |

`--land` also doubles as the ink of the misregistered second impression that
flashes while a land is pressed.

## Type

Baloo 2 800 for names and the wordmark; Nunito 900 for the small tracked caps
of the skill line. Both are inherited from the four games and are a brand
commitment — the whole site speaks in one voice.

- Land name: Baloo 2 800, `clamp(19px, 4.7vw, 25px)`, tracking `-.005em`.
- Skill line: Nunito 900, 10px (10.5px wide), tracking `.14em`, uppercase,
  `rgba(255,246,228,.86)` — tinted from the paper, never grey, and measured at
  5:1 against the lightest plaque.
- Wordmark: Baloo 2 800, 19px on a phone, 31px at 680px, 37px at 1200px.

## Structure

- `body` is fixed and `overflow:hidden`. **The picker never scrolls**, at any
  size. This is a product constraint, not a preference.
- `.board` is a grid filling the viewport, padded by `--key` plus the safe-area
  insets, with `--key` gaps. The ink behind it shows through as the keyline.
- Below 680px: one column, four rows of `minmax(0,1fr)`.
- From 680px: 2x2. This covers a phone held sideways as well as a desktop.
- `.land svg` fills its land with `preserveAspectRatio="xMinYMid slice"` —
  left-anchored so the character survives the crop at every aspect ratio. A
  land that needs to push in on a small hero sets `--zoom` and `--origin`
  rather than having its artwork redrawn.

### Where a plaque sits

The plaque goes wherever the character is not.

- Phone: top-left of each land, since every hero lives in the lower half —
  except Beacon Bridge, whose fox sits high, so its plaque takes the bottom.
- 680px and up: the four outer corners of the board (TL, TR, BL, BR), which
  clears the middle for the medallion.

### The medallion

One element, centred at `top:50% left:50%` on every screen. With four stacked
lands the middle of the board is the seam between the second and the third, so
the phone gets a 98px medallion straddling that seam; at 680px it grows to
158px, where the keylines genuinely cross, and gains its second line. It is
`pointer-events:none`, so the land beneath it stays tappable.

## Material

- **Grain.** `.board::after` lays a desaturated, stitched `feTurbulence` tile
  over everything at `opacity:.4; mix-blend-mode:overlay`. This is the material
  that makes the board read as printed rather than rendered; it is not a
  decorative flourish and must stay perceptible if it is retuned.
- **Die-cut plaques.** 2px paper keyline, 16px radius (19px wide), a hard
  `0 4px 0` ink drop plus a soft ambient shadow. On the two night lands the
  ambient shadow deepens, because a die-cut edge alone does not separate a dark
  label from dark art.
- **Playing pieces.** The token is a paper disc carrying a board pawn, 46px
  (50px wide) with a 23px glyph. It is a piece, not a media Play button.

## Motion

One authored moment, then nothing. Everything is inside
`@media (prefers-reduced-motion: no-preference)`.

- **Printing in.** Each land eases up 14px and settles, 70ms apart, on
  `cubic-bezier(.16,1,.3,1)`; the medallion stamps last.
- **Press.** The land's own ink flashes a second impression offset by 3px, the
  art scales 1.2%, the plaque and its piece press down into their shadows.
- **Hover** (fine pointers only) scales the art 5% and lifts the plaque.

## States

- Focus and keyboard selection share one printed frame: a 4px paper bracket
  inset 7px with an ink outline. **Never a glow.**
- Arrow keys move the selection — one step left/right, a row up/down when the
  board is 2x2 — and Enter opens the land. This path is inherited from the
  games and must keep working.
- Every land is one link. The whole land is the tap target.

## Rules

1. No card containers. A land is full-bleed art divided by a keyline; the
   plaque floats on it.
2. No gradients as type or as chrome. The art carries all the colour.
3. No glow. Depth is an offset plus a blur, or a hard printed drop.
4. The picker fits one screen. If something new must be added, something must
   give way.
5. A child who cannot read must still be able to choose. Art leads; the plaque
   confirms.

# Starfall Meadow

A 90-second browser catching game for 8-year-olds. Open `index.html` — that's it.
No build step, no server, no dependencies, no API keys.

## The game

Star-sprites are flung in big looping arcs across the sky. You are Pip, and you run
and hop underneath them to catch them before they land.

The whole design rests on one idea: **you cannot catch a star by chasing where it is.**
You have to read its arc and run to where it is *going to be*. A breeze bends the arcs,
so there is a second variable to read as well.

## Controls

| Key | Action |
| --- | --- |
| <kbd>←</kbd> <kbd>→</kbd> | Run left and right |
| <kbd>Space</kbd> | Hop — press twice for a flutter-jump |
| <kbd>↑</kbd> | Hop (same as Space) |
| <kbd>↓</kbd> | Dive down fast while in the air |
| <kbd>Space</kbd> | Start / play again on menus |
| <kbd>Q</kbd> | Open and close the Grown-Ups menu |

No mouse is used anywhere.

## What it builds

- **Trajectory prediction** — reading a curved path and extrapolating its end point.
- **Anticipatory timing** — committing to a movement *before* the target arrives, which
  is a different skill from raw reaction speed.
- **Visual-spatial reasoning** — holding two or three moving arcs in mind and planning a
  route that intercepts them in order.
- **Motor planning** — chaining run → hop → dive into one smooth intercept.
- **Sustained selective attention** — tracking the star that matters against a busy sky.

Pressing <kbd>Q</kbd> opens a menu written for parents that covers all of this, plus live
session stats and settings.

## How the difficulty adapts

The game keeps a rolling measure of catch accuracy.

- Doing well → arcs fly faster, the breeze picks up, and the **dotted flight-path guide
  fades away**.
- Struggling → the guide fades back in, arcs slow down, and gentle floaty bubble-stars appear.

A warm-up ceiling keeps the first ~14 seconds gentle regardless of a lucky opening streak,
and the guide never fully disappears in the first 15 seconds of a round.

Every star is placed by a solver that bisects on the *same integrator the game runs*, so
each one lands at an exact time and position that is provably reachable from where the
previous star landed at Pip's top speed. A miss is always a readable mistake, never bad luck.

## No failure state

Missed stars sink into the meadow and **bloom into flowers**. They are counted as a reward
on the results screen ("Flowers grown"). There is no "Game Over" anywhere in this game.

## Progression

Eight sky levels, earned by catching stars, shift the entire world through the day:

`Sunny Meadow → Breezy Hills → Golden Fields → Sunset Ridge → Rosy Dusk → Twilight Vale → Starry Night → Aurora Sky`

Difficulty and sky level are deliberately separate axes: the sky is a *reward* for playing,
difficulty responds to *skill*. A struggling child still gets to see the aurora.

## Technical notes

Everything is generated at runtime — there are no image or audio downloads.

- **Art**: procedural canvas 2D. Value-noise mountain ridges, baked cloud and flower
  sprites, a cached static backdrop, layered parallax, swaying grass, fireflies, aurora
  ribbons, and an additive bloom pass.
- **Audio**: Web Audio API. FM-synthesised bells whose pitch climbs with your streak, an
  algorithmic reverb built from a generated impulse response, and a pentatonic music bed
  whose density tracks the difficulty.
- **Performance**: gradients are baked into sprite caches, particles render in two batched
  passes with a single composite-op switch, and an adaptive quality controller sheds bloom
  then particle count if frame time slips.
- **Robustness**: the render loop is wrapped so a draw exception unwinds the canvas state
  and keeps running — a child should never be able to reach a frozen screen.

The only network request is a Google Fonts stylesheet for the rounded display face; the
font stack falls back cleanly to system rounded fonts offline.

`window.__SFM` exposes the game objects for debugging.
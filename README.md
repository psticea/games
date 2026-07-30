# games

Single-file browser games for kids. No build step, no server, no accounts, no API keys.

Open `index.html` for the picker, or jump straight to a game:

| Game | Folder | Ages | Round | Tech |
| --- | --- | --- | --- | --- |
| [Comet Catch — Bloop's Sky Carnival](comet-catch/index.html) | `comet-catch/` | ~6–10 | 75s | 3D, three.js via CDN |
| [Starfall Meadow](starfall-meadow/index.html) | `starfall-meadow/` | ~7–10 | 60s | 2D canvas, no dependencies |

Both use **arrow keys, Space and `Q` only** — no mouse — and neither has a "Game Over".

---

## Comet Catch — Bloop's Sky Carnival

A single-file 3D browser game for roughly ages 6–10. Open `comet-catch/index.html`.

**Play:** `←` `→` move · `Space` jump (and start/restart) · `↓` skid to a stop · `Q` grown-ups menu.

Bloop carries a basket. Three flower cannons fire comets in long, readable arcs; run to where each one will land and let it drop in. Rounds are 75 seconds by default.

### The skill it builds

The core mechanic *is* the exercise: **trajectory prediction and interception** — read a curved flight path early, decide where it will land, and get there first. That is coincidence–anticipation timing, the same skill used to catch a ball.

The game adapts. It tracks the last 12 comets as a rolling skill estimate that drives speed, spawn rate, travel distance and — most importantly — the **helper landing ring**. Beginners get a bright ring on the ground showing the landing spot; as the catch rate climbs the ring shrinks and fades to nothing, so the scaffolding is removed exactly as fast as the child stops needing it. In a bot playtest the estimate moved 0.36 → 0.93 while helper assist went 0.96 → 0.

Secondary skills: response inhibition (sneezy clouds must be *dodged*), second-order prediction (bouncy berries change path mid-flight), visual pattern recognition (each cannon glows in the colour of the comet it is about to fire), reaction time and spatial awareness.

No "Game Over", no lives, no red X. A miss is a puff of sparkles, and the next catch pays a Bounce Back bonus.

### Built with

three.js via CDN (WebGL, PCF soft shadows, ACES tonemapping, selective HDR bloom). Every texture, mesh, animation and sound is generated in-page — canvas-2D textures, procedural geometry, and a Web Audio synth for the marimba tones, whooshes and the adaptive pentatonic music loop. No sprite or audio files.

The renderer degrades in small ordered steps if a frame genuinely costs too much, measured as *our own work per frame* rather than wall-clock frame rate — so a 30 Hz display or a throttled tab never downgrades the art.

---

## Starfall Meadow

A 60-second 2D catching game for roughly ages 7–10. Open `starfall-meadow/index.html` — it has no runtime dependencies at all and works offline.

**Play:** `←` `→` run · `Space` hop (twice for a flutter-jump) · `↑` hop · `↓` dive · `Q` grown-ups menu.

Star-sprites are flung in big looping arcs across the sky. You are Pip, and you run and hop underneath them to catch them before they land.

### The skill it builds

The whole design rests on one idea: **you cannot catch a star by chasing where it is.** You have to read its arc and run to where it is *going to be*. A breeze bends the arcs, so there is a second variable to read as well.

- **Trajectory prediction** — reading a curved path and extrapolating its end point.
- **Anticipatory timing** — committing to a movement *before* the target arrives, which is a different skill from raw reaction speed.
- **Visual-spatial reasoning** — holding two or three moving arcs in mind and planning a route that intercepts them in order.
- **Motor planning** — chaining run → hop → dive into one smooth intercept.
- **Sustained selective attention** — tracking the star that matters against a busy sky.

### How the difficulty adapts

The game keeps a rolling measure of catch accuracy. Doing well → arcs fly faster, the breeze picks up, and the **dotted flight-path guide fades away**. Struggling → the guide fades back in, arcs slow down, and gentle floaty bubble-stars appear. A warm-up ceiling keeps the first ~14 seconds gentle regardless of a lucky opening streak.

Every star is placed by a solver that bisects on the *same integrator the game runs*, so each one lands at an exact time and position that is provably reachable from where the previous star landed at Pip's top speed. A miss is always a readable mistake, never bad luck.

### No failure state

Missed stars sink into the meadow and **bloom into flowers**. They are counted as a reward on the results screen ("Flowers grown"). There is no "Game Over" anywhere in this game.

Eight sky levels, earned by catching stars, shift the whole world through the day: `Sunny Meadow → Breezy Hills → Golden Fields → Sunset Ridge → Rosy Dusk → Twilight Vale → Starry Night → Aurora Sky`. Sky level rewards *playing*; difficulty responds to *skill*. A struggling child still gets to see the aurora.

### Built with

Plain canvas 2D and the Web Audio API — nothing else. Value-noise mountain ridges, baked cloud and flower sprites, a cached static backdrop, layered parallax, swaying grass, fireflies, aurora ribbons and an additive bloom pass. Audio is FM-synthesised bells whose pitch climbs with your streak, an algorithmic reverb built from a generated impulse response, and a pentatonic music bed whose density tracks the difficulty.

Gradients are baked into sprite caches and particles render in two batched passes, so the busiest moments stay cheap; an adaptive quality controller sheds bloom then particle count if frame time slips. The render loop is wrapped so a draw exception unwinds the canvas state and keeps running — a child should never be able to reach a frozen screen.

`window.__SFM` exposes the game objects for debugging.

# games

Single-file browser games for kids. No build step, no server, no accounts, no API keys.

**▶ [Play them here](https://psticea.github.io/games/)**

| Game | Play | Ages | Round | Tech |
| --- | --- | --- | --- | --- |
| Comet Catch — Bloop's Sky Carnival | [play](https://psticea.github.io/games/comet-catch/) · [`comet-catch/`](comet-catch/index.html) | ~6–10 | 75s | 3D, three.js via CDN |
| Starfall Meadow | [play](https://psticea.github.io/games/starfall-meadow/) · [`starfall-meadow/`](starfall-meadow/index.html) | ~7–10 | 60s | 2D canvas, no dependencies |
| Starbounce — Zip's Sky Rescue | [play](https://psticea.github.io/games/starbounce/) · [`starbounce/`](starbounce/index.html) | ~7–10 | <2 min a level | 2D canvas, no dependencies |
| Beacon Bridge | [play](https://psticea.github.io/games/beacon-bridge/) · [`beacon-bridge/`](beacon-bridge/index.html) | ~7–10 | ~60–90s a crossing | 2D canvas, no dependencies |

All four use **arrow keys, Space and `Q` only** — no mouse — and none of them has a "Game Over".
The root page is a picker linking to all four.

---

## Comet Catch — Bloop's Sky Carnival

A single-file 3D browser game for roughly ages 6–10. Open `comet-catch/index.html` — no build step, no server, no keys.

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

A 60-second 2D catching game for roughly ages 7–10. Open `starfall-meadow/index.html` — it has no runtime dependencies and works offline.

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

---

## Starbounce — Zip's Sky Rescue

An aim-and-launch game for roughly ages 7–10. Open `starbounce/index.html` — no runtime dependencies, works offline.

**Play:** `←` `→` aim the cannon (and steer the catcher cloud in flight) · `↑` `↓` power · `Space` launch · `Q` grown-ups menu.

Zip is a little star-comet. Aim a cannon, set its power, and launch Zip in an arc to pop the **golden rescue stars** and free the sky-friends trapped inside. Chain bubbles on the way for combos, bank off the candy walls, jelly blobs and candy rails, and steer the catcher cloud under Zip as it falls to save a hop. Levels are procedurally generated and always finish in under two minutes.

### The skill it builds

**Trajectory prediction and angle of reflection** — spatial reasoning, plus fine motor control from steering the cloud mid-flight while the arc is still playing out.

The interesting part is the scaffold. A sparkling guide-path shows exactly where Zip will fly, simulated with the *same integrator the ball actually uses*, so it never lies. After every shot the game scores how well that shot was predicted and shrinks the guide from **2.30 s down to 0.42 s** as accuracy climbs — the prediction migrates off the screen and into the child's head. Miss a few and the scaffold grows straight back, an invisible grace radius on the rescue stars widens, and the catcher cloud gets wider (78 → 112 px). The skill estimate persists in `localStorage`, and the grown-ups menu shows the live readout ("guide reduced 46%").

Star placement follows the same curve: early levels put rescue stars in the easy central band, later levels push them to the edges and the ceiling so banking becomes necessary.

### No failure state

Running out of hops refills them (twice on the first two levels, once after that) with a **BONUS HOPS** fanfare. If a level really does end, the screen says "SO CLOSE! Every try makes your Star Sense stronger", offers a tip, and re-rolls the board one step easier with two extra hops — never the identical layout twice.

### Built with

Plain canvas 2D and the Web Audio API. Clouds, floating islands, glass bubbles, gold stars, jelly bumpers, candy rails and the aurora sky are all drawn from gradients and baked into offscreen sprite canvases at load, then blitted. Audio is a major-pentatonic combo scale (nothing can sound wrong), a convolution reverb built from generated noise, and a I–V–vi–IV arpeggio bed that speeds up during fever.

Bloom is a downsample pyramid (÷3 → ÷6 → ÷12) composited additively rather than a per-frame blur filter, about 4× cheaper. Particles use a compact pool drawn in two batched passes. An adaptive quality governor watches a frame-time EMA and scales burst sizes or drops the particle glow pass. Worst case measured at ~10 ms/frame with 325 live particles in software-rendered headless Chromium.

Levels are seeded from the level number (mulberry32) and laid out with one of seven patterns: grid, arcs, flower, spiral, wave, pillars, diamond. Every rescue star on levels 1–12 *and* their retry re-rolls was verified reachable by brute-forcing 70 angles × 18 powers through the real physics, so no level can soft-lock.

`window.__G` exposes the game state for debugging.

---

## Beacon Bridge

A mental-rotation puzzle for roughly ages 7–10. Open `beacon-bridge/index.html` — no runtime dependencies, works offline.

**Play:** `←` `→` slide the block · `↑` `↓` turn it a quarter turn · `Space` drop it into the gap · `Q` grown-ups menu.

A fox needs to cross a canyon at dusk. Each of the five stone spans has a piece missing, and a wooden block floats above it in the wrong orientation. Turn the block, line it up, and drop it. A whole span means the fox trots to the next one; five spans lights the beacon on the far cliff. A crossing takes about 60–90 seconds.

### The skill it builds

**Mental rotation** — picturing what a shape will look like once it is turned, *before* committing to it. It is one of the most-studied spatial abilities and the one that later carries geometry, map reading, packing, engineering and design. The mechanic *is* the exercise: nothing else stands between the child and the gap.

- **Mental rotation** — matching a rotated silhouette to a target outline.
- **Spatial visualisation** — holding the block and the hole in mind at once and comparing them without moving anything.
- **Plan-then-act** — the drop is instant, so all the thinking happens first. A first-try fit earns a star.

The block never starts in the solved orientation, and the check is by cell-set identity rather than rotation index, so a symmetric shape can't hand out a free "already solved" start.

### How the difficulty adapts

The game counts attempts per block. Across a crossing, an average at or under 1.35 tries moves the level up; 2.4 or more moves it down, clamped to 1–6. Level chooses the shape pool — trominoes → tetrominoes → pentominoes — and how much slack the stone window has around the shape (1 spare column early, 2 later, so there are more wrong places to line it up). Spans 4 and 5 of every crossing are drawn one level harder than the rest, so each crossing still ramps.

After three tries on the same block, a soft glowing outline appears inside the gap showing exactly where it should sit. There is no timer, no lives and no "Game Over" — a wrong block simply comes to rest on the stone, where the child can *see* why it missed, and floats back up.

### Why it can never soft-lock

The gap is carved from a rotation only if every column of that rotation is a run of cells starting at the top row (`topOpen`). Otherwise stone would sit above a hole and gravity could never deliver the block into it. Shapes with no valid rotation (the S and Z tetrominoes) and shapes with fewer than two distinct rotations (the O tetromino) are filtered out of the shape table at load. The stone window is always at least as wide as the widest rotation, so no orientation can ever get wedged against a wall.

Because a rotation always has the same cell count as the gap, "every dropped cell landed in a hole" is equivalent to an exact fill, which makes the fit test both cheap and exact.

### Built with

Plain canvas 2D and the Web Audio API. The dusk canyon — sky gradient, mesa silhouettes, layered ridges, arch substructure, mist bands and vignette — is drawn from gradients and hash-noise, and the fox is authored from curves so it walks, breathes and blinks. The block is always drawn as its rotation-0 cell set spun about its own bounding-box centre, so the silhouette stays exact through every intermediate angle of the turn tween. Audio is a small synth: soft filtered tones for turns and slides, a woody knock on landing, a warm stacked chord when a span completes and a C-major arpeggio when the beacon lights, over a bed of low-passed brown noise standing in for canyon wind. No music loop, and the whole mix sits at a quarter gain.

The render loop wraps update and draw so a stray exception unwinds the canvas state and keeps running — a child should never be able to reach a frozen screen.

`window.__BB` exposes the game state for debugging.

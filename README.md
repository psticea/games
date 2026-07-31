# games

Single-file browser games for kids. No build step, no server, no accounts, no API keys.

**▶ [Play them here](https://psticea.github.io/games/)**

| Game | Play | Ages | Round | Tech |
| --- | --- | --- | --- | --- |
| Comet Catch — Bloop's Sky Carnival | [play](https://psticea.github.io/games/comet-catch/) · [`comet-catch/`](comet-catch/index.html) | ~6–10 | 75s | 3D, three.js via CDN |
| Starfall Meadow | [play](https://psticea.github.io/games/starfall-meadow/) · [`starfall-meadow/`](starfall-meadow/index.html) | ~7–10 | 60s | 2D canvas, no dependencies |
| Starbounce — Zip's Sky Rescue | [play](https://psticea.github.io/games/starbounce/) · [`starbounce/`](starbounce/index.html) | ~7–10 | 90s | 2D canvas, no dependencies |
| Beacon Bridge | [play](https://psticea.github.io/games/beacon-bridge/) · [`beacon-bridge/`](beacon-bridge/index.html) | ~7–10 | 90s a crossing | 2D canvas, no dependencies |
| Foxglove Trail | [play](https://psticea.github.io/games/foxglove-trail/) · [`foxglove-trail/`](foxglove-trail/index.html) | ~7–10 | 3 nights, <2 min | 2D canvas, no dependencies |

All five use **arrow keys, Space and `Q` only** — no mouse — and none of them has a "Game Over".
The root page is a picker linking to all five.

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

A 90-second aim-and-launch game for roughly ages 7–10. Open `starbounce/index.html` — no runtime dependencies, works offline.

**Play:** `←` `→` aim the cannon · `↑` `↓` power · `Space` launch · `Q` grown-ups menu.

Zip is a little star-comet. Aim a cannon, set its power, and launch Zip in an arc to pop the **golden stars** and free the sky-friends inside. Bank off the candy walls, jelly blobs and candy rails to reach the awkward ones. Popped stars are instantly replaced, so the sky never empties and there is always another shot worth taking.

One round is 90 seconds and that is the whole game — no levels, no lives, no progression to grind. When the clock runs out you get a score, a 1–3 star rating and a single Space press to go again.

### The skill it builds

**Trajectory prediction and angle of reflection** — spatial reasoning, plus the fine motor control of holding an arrow key for exactly the right fraction of a second.

The interesting part is the scaffold. A sparkling guide-path shows exactly where Zip will fly, simulated with the *same integrator the ball actually uses*, so it never lies. The game scores how well each shot was predicted and shrinks the guide from **2.30 s down to 0.42 s** as accuracy climbs — the prediction migrates off the screen and into the child's head. Miss a few and the scaffold grows straight back, an invisible grace radius on the stars widens, and the guide returns. The skill estimate persists in `localStorage`, and the grown-ups menu shows the live readout ("guide reduced 46%").

New stars also drift toward trickier positions — closer to the walls and the ceiling — as the rescue count climbs, so banking becomes necessary rather than optional.

### No failure state

There is nothing to lose. Missing costs only a couple of seconds, the sky refills itself, and the round always ends the same friendly way. The only resource is the clock.

### Built with

Plain canvas 2D and the Web Audio API. Clouds, floating islands, golden stars, jelly bumpers, candy rails and the aurora sky are all drawn from gradients and baked into offscreen sprite canvases at load, then blitted. Audio is a major-pentatonic combo scale (nothing can sound wrong), a convolution reverb built from generated noise, and a I–V–vi–IV arpeggio bed that speeds up during a combo streak.

Bloom is a downsample pyramid (÷3 → ÷6 → ÷12) composited additively rather than a per-frame blur filter, about 4× cheaper. Particles use a compact pool drawn in two batched passes. An adaptive quality governor watches a frame-time EMA and scales burst sizes or drops the particle glow pass.

Physics runs on a clamped timestep so a stutter can never tunnel Zip through a wall, while the round clock runs on real elapsed time so "90 seconds" means 90 seconds even on a slow machine. Leaving the tab pauses the round rather than draining it.

Every arena is generated fresh, then a fan of real trajectories is swept once to work out which star positions a shot can actually reach — stars only ever spawn in those, so the game cannot present a target that is impossible to hit.

`window.__G` exposes the game state for debugging.

---

## Beacon Bridge

A mental-rotation puzzle for roughly ages 7–10. Open `beacon-bridge/index.html` — no runtime dependencies, works offline.

**Play:** `←` `→` slide the block · `↑` `↓` turn it a quarter turn · `Space` drop it into the gap · `Q` grown-ups menu.

A fox has to cross a canyon before the light goes. Each of the three stone spans has a bite taken out of its deck, and **two** wooden blocks are needed to make it whole again — a lower one that slots into the notch, then an upper one that caps it off flush with the road. Three spans, six blocks, ninety seconds. Fill all three and the beacon on the far cliff lights up.

The next block is always shown in the corner, so the first drop has to be chosen with the second one already in mind. If the finished span is wrong, **both** blocks float back up and the span is offered again — the section restarts, nothing else is lost.

### The skill it builds

**Mental rotation** — picturing what a shape will look like once it is turned, *before* committing to it. It is one of the most-studied spatial abilities and the one that later carries geometry, map reading, packing, engineering and design. The mechanic *is* the exercise: nothing else stands between the child and the gap.

- **Mental rotation** — matching a rotated silhouette to a target outline.
- **Two-step spatial planning** — holding both blocks and the notch in mind at once, and working out which has to go in first. That is working memory and forward planning rather than trial and error.
- **Spatial visualisation** — comparing shape to hole without moving anything.

Neither block ever starts in its solved orientation, and the check is by cell-set identity rather than rotation index, so a symmetric shape can't hand out a free "already solved" start.

### How the difficulty adapts

There are six steps, and the level is remembered in `localStorage` between sittings. It never changes mid-crossing — only when the next bridge is started.

| Step | Deck width | Lower block | Upper block |
| --- | --- | --- | --- |
| 1 | 5 cells | trominoes | trominoes |
| 2 | 5 | + tetrominoes | trominoes |
| 3 | 6 | + tetrominoes | + tetrominoes |
| 4 | 6 | tetrominoes | tetrominoes |
| 5 | 7 | tetrominoes | tetrominoes |
| 6 | 7 | tetrominoes, `S` `Z` `T` weighted double | tetrominoes |

A wider deck means more columns to line a block up in; bigger blocks mean an eight-cell notch instead of six. The ladder is monotonic on every axis it moves — measured over 600 generated spans per step, average notch size goes 6.0 → 6.7 → 7.5 → 8.0 → 8.0 → 8.0, the share of four-cell blocks goes 0% → 34% → 74% → 100% → 100% → 100%, and the share of the awkward `S`/`Z` blocks climbs 0% → 16% → 20% → 26% → 28% → 32%. No two steps are the same and none of them goes backwards.

At the end of a crossing the game averages the attempts across the three spans (one attempt = built first go). An average of 1.34 or better moves up a step — so a single retry anywhere still earns a promotion — and 2.0 or worse moves down, as does running out of daylight. One step per crossing, never more. The **last span of every crossing is drawn one step harder** than the other two, so each crossing ramps on its own.

The grown-ups menu shows the current step and what it means, so none of this is hidden.

After two goes at the same span, a soft glowing outline shows exactly where the current block belongs. There is no "Game Over": running out of light ends the walk gently at dusk and offers another go.

### The eight blocks, and why a solution always exists

Two trominoes and six tetrominoes — `I3 L3 I4 L4 J4 T4 S4 Z4`. Every one has at least two genuinely different silhouettes, so turning always matters; the square is left out precisely because it doesn't.

Each notch is **built rather than guessed**. The upper block is laid flush with the deck surface, and the lower block is then slotted directly beneath it at one consistent vertical offset across every column it uses. That construction guarantees three things at once: every gap column runs unbroken from the deck surface downwards, so a block can always fall into it; the lower block, dropped first, comes to rest exactly where it was laid, because the stone beneath stops it; and the upper block, dropped second, comes to rest exactly on top of the lower one. A solution always exists and gravity can always reach it.

The upper block additionally has to be flat-topped in its chosen orientation — every column running from its own top row down — or stone would sit above it and it could never be dropped in. That is why `S4` and `Z4` only ever appear as the lower block. Generation is then verified against the game's own physics before a span is handed over. Across 2,400 generated spans (400 at each of the six levels) every one was solvable, every one had both blocks starting in the wrong orientation, and none took more than a fraction of a millisecond to build.

Because two blocks always have the same total cell count as the notch, "every placed cell landed inside the notch" is equivalent to an exact fill, which makes the completion test both cheap and exact.

### Built with

Plain canvas 2D and the Web Audio API. The dusk canyon — sky gradient, sinking sun, star field, three layers of ridge, strata, river, mist, drifting fireflies — is drawn from gradients and value noise, and the fox is authored from curves with two-bone IK legs so he has real knees, a brush tail that lags behind the body, ears that flick and a head that counter-bobs against his gait. The floating block is always drawn as its rotation-0 cell set spun about its own bounding-box centre, so the silhouette stays exact through every intermediate angle of the turn.

**The 90-second clock is the sky.** Daylight drains from a warm late afternoon to full night over the round: the sun sinks behind the far ridge, stars come out, the lanterns along the parapet light themselves and fireflies rise out of the gorge. Two whole palettes are interpolated per frame, so the clock is something a child can feel rather than read.

Every stone and wood face is baked into an offscreen tile and blitted, and the ridges and strata are built once as `Path2D` and drawn under a translate, which took the frame from ~30 ms to ~20 ms in software-rendered headless Chromium. Audio is a small synth — soft filtered tones for turns and slides, a woody knock on landing, a warm chord when a span closes and a C-major arpeggio when the beacon lights — over a bed of low-passed brown noise standing in for canyon wind.

The render loop wraps update and draw so a stray exception unwinds the canvas state and keeps running — a child should never be able to reach a frozen screen.

`window.__BB` exposes the game state for debugging.

---

## Foxglove Trail

A map-reading game for roughly ages 7–10. Open `foxglove-trail/index.html` — no runtime dependencies, works offline.

**Play:** `←` `↑` `↓` `→` run · `Space` sniff (and start/continue) · `Q` grown-ups menu. Those are the only keys — no mouse.

A fox has to get home to the den before the evening fog closes in. You can only see a few paces of wood — but a paper map in the corner shows the **whole** forest, with the den marked by a dotted ring. A round is three short nights and takes under two minutes.

### The skill it builds

The mechanic *is* the exercise: **allocentric → egocentric translation**, the real skill behind map reading and orienteering. The map does not turn when you do. On later nights it is pinned at an angle, so "the den is up-left on the paper" has to be converted into "so I run *that* way from here" — mental rotation of a whole layout, performed while steering.

- **Landmark-based wayfinding** — a fallen log, standing stone, berry bush, mushroom ring and pond are drawn *both* in the wood and on the map, each with a distinct silhouette. Every junction carries one, and walking near a landmark rings it on the paper so the two pictures link up.
- **Route planning and self-location** — sniffing pulses your position onto the map but recharges slowly, so a child learns to keep a running sense of where they are instead of checking constantly.
- **Spatial working memory** — the map is studied on the briefing screen and stays small during play.

### How it adapts

The game scores how *directly* each night was walked (shortest path ÷ distance actually covered) and keeps a rolling skill estimate in `localStorage`. Every scaffold is tied to it and is removed one at a time:

| Scaffold | Beginner | Confident |
| --- | --- | --- |
| Map rotation | 0° on night 1 | up to 155° |
| Circle of sight | 330 px | 232 px |
| Sniff recharge | 3.8 s | 7.2 s |
| Breadcrumb trail on the paper | on | off |
| "Screen-up" arrow on the map rim | on | faded out |
| Wood size | 5×3 grid | up to 7×4 grid |

The grown-ups menu shows the live readout of all of it.

### No failure state

If the fog closes in completely, an owl starts calling from the den — **stereo-panned, so you can hear which side it is on** — a warm glow appears at the screen edge pointing home, and the moon slowly comes out so the circle of sight *widens* the longer a child takes. The night always finishes. The results card says how straight the trail was read, never how badly.

### Built with

Plain canvas 2D and the Web Audio API. Every tree, fern, landmark, foxglove and the den are procedurally drawn once into offscreen canvases at load, then trimmed to their alpha bounds and blitted. Woods are generated with a jittered grid, a Kruskal minimum spanning tree plus extra loop edges, and Dijkstra to choose a start node at the target path length — so the wood is always connected and the den is always reachable. The map is a separate ink-on-aged-paper rendering scaled to the trail's bounding box.

Audio is a brown-noise wind bed, FM-free sine voices through a generated-impulse convolution reverb, a pentatonic pad whose density tracks the fog, and a vibrato owl hoot panned toward the den.

The renderer only draws what the circle of sight can reach: the ground blit, sprite culling and the fog itself are all clipped to a quantised box around the fox, and the fog disc is cached until its radius changes. That took the frame from 216 ms to 27 ms in software-rendered headless Chromium. The fox is always drawn last and anything that would cover her fades out, so she can never be lost behind a canopy.

`window.__FG` exposes the game state for debugging.

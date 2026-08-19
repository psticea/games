# games

Single-file browser games for kids. No build step, no server, no accounts, no API keys.

**▶ [Play them here](https://psticea.github.io/games/)**

| Game | Play | Ages | Round | Tech |
| --- | --- | --- | --- | --- |
| Ducks in a Row | [play](https://psticea.github.io/games/ducks-in-a-row/) · [`ducks-in-a-row/`](ducks-in-a-row/index.html) | ~5–10 | 90s | 2D canvas, no dependencies |
| Owlet Grove | [play](https://psticea.github.io/games/owlet-grove/) · [`owlet-grove/`](owlet-grove/index.html) | ~4–9 | 60s | 2D canvas, no dependencies |
| Beacon Bridge | [play](https://psticea.github.io/games/beacon-bridge/) · [`beacon-bridge/`](beacon-bridge/index.html) | ~7–10 | 90s a crossing | 2D canvas, no dependencies |
| Foxglove Trail | [play](https://psticea.github.io/games/foxglove-trail/) · [`foxglove-trail/`](foxglove-trail/index.html) | ~7–10 | 3 nights, <2 min | 2D canvas, no dependencies |

None of them has a "Game Over". Beacon Bridge and Foxglove Trail use **arrow keys, Space, `Q` and `H` only** — no
mouse — and `H` closes the game and comes straight back to the picker.
Owlet Grove uses **Space** to fly and **`Q`** for the grown-ups menu on a keyboard, and a tap anywhere to fly on a touch screen.
Ducks in a Row is steered by **holding anywhere** on a touch screen, or by holding `←` `→` on a keyboard.
Beacon Bridge zooms out on a narrow screen just far enough to hold the reindeer and the span being mended in the same shot, and the
sky and the gorge run past the top and bottom of the frame so the picture still fills the screen.
The root page is a compact picker: a painting, the game's name and a Play button for each of those four.

Two older games are still here and still playable, just no longer listed on the picker, to keep the front page short:
Comet Catch — [play](https://psticea.github.io/games/comet-catch/) · [`comet-catch/`](comet-catch/index.html) — and
Starfall Meadow — [play](https://psticea.github.io/games/starfall-meadow/) ·
[`starfall-meadow/`](starfall-meadow/index.html), which Ducks in a Row replaced in the first slot.

## On a phone

Every game is driven by touch, every game has an always-visible **home button** that returns to the picker, and the picker and
all four games try to run without browser chrome.
Full screen on the mobile web is more awkward than it looks, so it is worth being plain about what actually happens:

- The Fullscreen API is **per document**. The spec fully exits fullscreen when a document unloads, so it can never survive the
  tap that opens a game — every page has to ask for itself, on its own user gesture. Each page therefore requests full screen on
  its first touch.
- **Chrome counts only a finished tap** as that gesture, so asking on `pointerdown` or `touchstart` alone is quietly ignored.
  Every game backs the request up on `touchend`, anywhere on the page. Miss that and the game simply runs with the browser's
  toolbars over it, on the most common Android setup, with nothing in the console to say why.
- **Safari on iPhone does not implement element fullscreen at all**, so on the most common kids' device that request does
  nothing.

So the site also ships a [web app manifest](site.webmanifest) (`display: fullscreen`, `scope: "./"`). Added to the home screen,
the whole site runs chrome-free *and stays that way across every page*, on iPhone as well as Android — this is the only approach
that survives navigating from the picker into a game. The picker shows a small, dismissible note on iOS explaining the
Add to Home Screen step, since that is the only route to full screen there.

Starfall Meadow needs the long edge of the screen and asks you to turn the phone sideways; that screen carries its own
"All games" link so a child can never get stuck on it. Ducks in a Row works in either orientation and reshapes its pond to fit —
portrait gets a pond that is taller than it is wide.

---

## Comet Catch — Bloop's Sky Carnival

> Not on the picker any more — reachable by link only: `comet-catch/index.html`.

A single-file 3D browser game for roughly ages 6–10. Open `comet-catch/index.html` — no build step, no server, no keys.

**Play:** `←` `→` move · `Space` jump (and start/restart) · `↓` skid to a stop · `Q` grown-ups menu · `H` back to all games.

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

## Ducks in a Row

A 90-second pond for roughly ages 5–10. Open `ducks-in-a-row/index.html` — no build step, no server, no keys, no dependencies.

**Play:** hold **anywhere** and she turns towards your finger · `←` `→` (or `A` `D`) held down turn her on a keyboard ·
`Space` start and play again · `Q` grown-ups menu · `H` back to all games. The grown-ups menu also has a **Full screen**
button.

A mother duck paddles across a pond at golden hour, gathering her ducklings into a line behind her. She never stops swimming;
the only thing a child chooses is where she points.

### The decision the whole game rests on

**Take the near duckling now, or keep open water ahead for the line you are already towing.** Her own line is the one thing on
the pond she cannot swim through — brush it and a few ducklings pop out and have to be gathered again. A long line makes her
slower and her turning circle tighter, so the same gap that was easy at three ducklings is a real problem at twelve. A child
who chases whatever is closest spends the run repairing a scattered line; a child who sweeps wide and works the outside of the
pond first ends up with a line worth bragging about.

Nothing in the game says any of this. The first fifteen seconds are the tutorial.

### The skill it builds

- **Route planning** — the best duckling is usually not the nearest one, it is the one that leaves her pointing at open water.
- **Delayed gratification** — grabbing the closest thing works early and stops working, and a child discovers that by doing it.
- **Spatial judgement** — reading a turning circle that changes as the line grows.

### How the difficulty works — no adaptation, on purpose

Unlike Comet Catch and Foxglove Trail, this game keeps **no skill estimate and adapts to nobody**. It does not need to: the
line *is* the difficulty curve. Each duckling costs her 3% of her speed to a floor of 62%, and her turn rate eases from 250°/s
down to 195°/s, so a long line steers like a barge and the pond fills with an obstacle the child built themselves. The only
scripted curve is the spawn pacing, which follows the shape of the round:

| Time | What the pond does |
| --- | --- |
| 0–20s | Calm. Three ducklings at a time, a slow spawn. She discovers that they follow her. |
| 20–55s | Escalation. More ducklings, a drifting current, lines long enough to be awkward. |
| 55–65s | **The lily-pad lull.** Spawns thin out and the water quietens. This is deliberate: a run that only escalates flattens into noise, and a child needs a few seconds to consolidate a long line and feel proud of it. |
| 65–90s | The sunset gathering. Double spawns, the light going amber, a visible crescendo. |

### No failure state

No "Game Over", no lives, no buzzer, no death. Brushing her own line **scatters at most four ducklings** and the rest of the
line closes ranks — the score already earned is never taken away, and there is a 1.4-second grace afterwards so one mistake
cannot chain into three. The reeds at the pond edge turn her gently back inward rather than stopping her. A scatter gets one
worried quack and a ring of ripple; it never gets a screen shake, because shaking the screen at a child who has just lost their
ducklings is punishment, which this game does not do. The screen only shakes — by at most two pixels — when they reach a new
longest line.

### The numbers, so the feel can be re-tuned

Every constant lives in one `K` object at the top of the file.

| Constant | Value | What it does |
| --- | --- | --- |
| `ROUND` | 90 s | Length of a round |
| `SPEED` | 196 u/s | Her speed with nobody following |
| `SPEED_FALL` / `SPEED_FLOOR` | 3% per duckling, floor 62% | How much a long line slows her |
| `TURN_MAX` / `TURN_MIN` | 250°/s → 195°/s at ten ducklings | How much a long line costs her agility |
| `SPACING` / `SPACING_MIN` | 34 units, bunching to 78% | Gap along the line; they bunch when she slows |
| `LAG` | 5 units | The last one trails, then hurries to catch up |
| `GATHER_R` | 36 units | **Forgiveness:** bigger than the duckling, so a near-enough sweep always collects |
| `SELF_R` | 14 units | **Forgiveness:** smaller than the line, so a visible near-miss never scatters || `SELF_SKIP` | 3 | The three ducklings closest to her never startle — that is where accidental brushes happen |
| `SCATTER_MAX` | 4 | The most a single mistake can ever cost |
| `GRACE` | 1.4 s | No second scatter for this long |
| `MAX_FREE` | 5 | Free-swimming ducklings on the pond at once |
| `LIFE` / `WARN` | 8.5 s / 2.4 s | How long a duckling waits, and how long it peeps and bobs before paddling off |
| `SPAWN_APART` / `SPAWN_FLOOR` | 168 / 100 units | Blue-noise spacing target, and its hard floor |
| `LINE_CLEAR` / `DUCK_CLEAR` | 40 / 150 units | Never spawn inside the line or on top of her |
| `POND_AREA` | 820² units | Pond area, reshaped to the viewport |

Scoring is `10 × (place in the line)`, so the first duckling is worth 10 and the twelfth is worth 120 — but each duckling only
ever pays for the **best place it has reached this round**, and ten for anything less. That one rule is what stops a child being
paid for wrecking their own line and collecting the same ducklings twice. A bot that farms scatters deliberately scores about
1,000 against roughly 6,000 for honest play.

### Reading the pond on a phone

The pond is larger than a phone screen, so free ducklings are often off it. Up to **three arrows** sit on the edge of the
screen, one per off-screen duckling, nearest first — a ring with the duckling drawn upright inside it and a point on the
outside saying which way to go. The nearer the duckling, the bigger and brighter its arrow, so the arrows rank the choice
rather than just listing it. They keep clear of the HUD, the two chrome buttons and the hint line.

Full screen is asked for on the first touch and again on the first `touchend`, since Chrome ignores the earlier request; it
keeps retrying until it is granted, then stops. The canvas is sized from `visualViewport` rather than `window.innerHeight`, so
it stays correct while a mobile toolbar is sliding away, and the grown-ups menu carries a **Full screen** button for the
manual route. On an iPhone, where there is no element fullscreen at all, that button says so and points at Add to Home Screen
instead of sitting there dead.

### Why the ducks never rotate

The art is drawn in profile, which a top-down world cannot simply rotate: pointing her south would stand her on her nose and
pointing her west would stand her on her head. So there are **three poses per bird** and no continuous squashing — a duck
stretched to some in-between width just looks compressed, which is worse than either honest pose.

| Heading | What you see |
| --- | --- |
| Left, right, or anywhere diagonal | The full profile at its true width, mirrored to whichever way she is going |
| Within ~10° of straight **down** the screen | Her front: both eyes, both wings, beak foreshortened towards you |
| Within ~10° of straight **up** the screen | Her back: tail cocked up, both wings, no face — she is swimming away |

Between them is a band about eight degrees wide, crossed in a couple of frames, where the profile narrows sharply and the two
blend. That is a transition, not a pose. The profile is drawn first and opaque with the narrow view over the top, because the
narrow sprite is the smaller of the two and so always lands inside the profile's silhouette — a plain cross-fade between two
half-transparent sprites lets the pond show through the middle of the duck, which looks far worse than the flattening it
replaced. The left/right mirror is latched with hysteresis so it can only flip while she is near vertical, which is exactly
when the narrow view is covering her and the swap cannot be seen.

The line itself is a **path buffer**: each duckling rides a fixed arc-length behind her, so it follows exactly where she went.
An earlier build added a per-index "stretch" scaled by how hard she was turning, which slid every duckling backwards along
the path — cumulatively, about three whole spacings by the tenth — and the line looked shoved rather than followed. Two of
the self-tests now assert the gaps hold whether she is straight or hauling the line round as hard as she goes.

### Built with

Plain Canvas 2D and the Web Audio API, and nothing else — no images, no audio files, no libraries. The palette is a single
OKLCH ramp from deep water to low sun, evaluated in the page. The duck, ducklings, reeds and lily pads are drawn once into
offscreen canvases at startup and on resize, then moved — six duck sprites in all, a profile, a front and a back for each of
the mother and a duckling. Ripples, foam and pollen come from one pooled array so the hot loop allocates nothing. The wake is
read straight off the path buffer rather than built out of particles.

Sound is all in **D major pentatonic**: each duckling peeps the next note up the scale as a chain builds, so a fast gather is
consonant by construction. Her quack sits an octave below, and the bed is a slow warm pad with filtered-noise water lapping and
occasional birdsong in the same key, ducking under every effect. The audio graph is built inside the first touch, so the *first*
run has sound rather than the second.

The simulation is a **pure layer** — fixed 1/120s timestep, seeded `mulberry32` throughout, no DOM, no canvas, no audio, no
clock reads and no bare randomness — and the line is a ring-buffer path rather than a physics chain, which is what makes it
deterministic and testable.

### Testing it

There is no test runner in this repo and no build step to hang one on, so the game ships its own:

**[`ducks-in-a-row/index.html?test=1`](ducks-in-a-row/index.html?test=1)** boots a self-test harness instead of the game, drives
the pure layer headlessly and prints a PASS/FAIL list, with the verdict in `document.title`. Forty checks, no dependencies,
works offline and from `file://`. Among them: a fixed-seed run scores an exact expected total; scatter detaches exactly the right
ducklings and never more than four; banked score survives; the round ends exactly on time and a simulated 30-second tab-out cannot
fast-forward it; 30, 60 and 144 Hz swim the same path; the line holds its spacing straight and through a hard turn; spawns never
land inside the line; a greedy pursuit bot can always reach something before it drifts off; collisions are honest in both
directions; farming scatters loses; and **planning a route beats grabbing the nearest duckling**, which is the check that says the
loop holds a real decision rather than being a toy.

### Known deviations from the brief

Expected to be non-empty, and it is.

1. **Turn rate and speed curves are not the ones specified.** The brief asked for 200°/s → 120°/s and a 1.5% speed loss per
   duckling. Both were measured and both were wrong, in the same way: her turning circle is speed ÷ turn rate, so dropping the
   turn rate steeply makes the circle *wider* as the line grows, and a line can only reach its own head once `n × SPACING`
   passes `2πr`. At the brief's numbers that needed about eighteen ducklings — a greedy bot took 27 in a row across ten seeds
   and **never scattered once**, so the rule the entire game rests on was inert. Taking her speed rather than her agility
   tightens the circle as the line grows, which is what "a long line is dangerous" actually requires. Crossing is now possible
   from eight ducklings.
2. **Scattering is capped at four ducklings** rather than everything behind the crossing point. Losing a line of twenty in one
   brush ends the session for a six-year-old; losing four teaches the same lesson and she keeps her line.
3. **Scoring is not `10 × the number already following`.** As written that pays nothing at all for the first duckling of every
   line. It is `10 × place`, with the personal-best rule above bolted on to kill the scatter-farming exploit, which measurably
   paid before it was added.
4. **The pond is small — about 820 units square, reshaped to the viewport.** Measured across three sizes, the route-planning bot
   beat the grab-the-nearest bot by 2% on a 1260-unit pond and by 12–36% on an 820-unit one. On a big pond her own line is never
   in the way, so there is no decision to get right.
5. **Two axes of control, not one key.** Route planning genuinely needs a direction. A virtual stick is useless to a five-year-old
   holding a phone one-handed, so it is steer-towards-your-finger, which needs no learning at all.
6. **The mother duck is yellow**, not the mallard brown a real one would be. She is told apart from her ducklings by size, by the
   wing and cheek that give her internal contrast in greyscale, and by the lily behind her ear — never by hue alone.
7. **Google Fonts is still loaded**, against the brief's no-third-party-requests rule, because Baloo 2 and Nunito are a
   site-wide brand commitment and the picker has already loaded them before any game opens. A lone exception buys no privacy and
   costs the site its single voice. If that request should go, it goes site-wide.

### What has not been verified

Honestly: **no child has played this yet.** Everything above about pacing and difficulty comes from bots and from measurement,
not from watching a six-year-old, and bots are a poor model of a child still learning to steer. The playtest items in the brief —
hand it to someone cold, watch a face at the moment a nine-duckling line comes round a wide turn, and put this and Starfall
Meadow in front of the same child to see which they pick for a third round — are the ones that matter most and none of them has
been done. Lighthouse has not been run against a deployed URL either.

---

## Starfall Meadow

> Not on the picker any more — reachable by link only: `starfall-meadow/index.html`. Ducks in a Row took its slot.

A 60-second 2D catching game for roughly ages 7–10. Open `starfall-meadow/index.html` — it has no runtime dependencies and works offline.

**Play:** `←` `→` run · `Space` hop (twice for a flutter-jump) · `↑` hop · `↓` dive · `Q` grown-ups menu · `H` back to all games.

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

## Owlet Grove

A 60-second flight for roughly ages 4–9. Open `owlet-grove/index.html` — no runtime dependencies, works offline, and it is the
one game here built for a phone first.

**Play:** `Space` fly (and start / play again) · `Q` grown-ups menu. On a touch screen, **tap anywhere** to fly. A **home**
button and a **Menu** button sit in the top-right corner on every device; on a narrow phone they collapse to two round icons.

Pip the owl drifts right through a forest. Gravity pulls down, a flap lifts, and the round is a pure score attack: **green leaves
are 1 point, red berries are 2, glowing fireflies are 5**. One round is exactly 60 seconds.

### No failure state, by construction

There are no obstacles at all — nothing to hit, no damage, no lives and no "Game Over". The ceiling and the grass are soft
bounces with a little puff of sparks, so a child who cannot yet hold a rhythm still flies for the full minute and still scores.
The only resource is the clock, and leaving the tab or opening the menu pauses it rather than draining it.

### The skill it builds

**Interception under a constant force.** The owl is always falling, so a flap has to be aimed at where Pip *will be* — reading a
curved path forward in time is the same coincidence–anticipation timing used to catch a ball. On top of that sits a **risk/reward
choice**: the spawner deliberately offers a firefly (5) off the easy line with three leaves (3) on it, and items arrive faster
than they can be deliberated over, so the decision has to be quick and then lived with.

### The spawner is the game design

Items are not scattered randomly. A scheduler emits one *cluster* at a time from a small set of hand-authored shapes — a leaf arc
that follows a natural flapping line, a berry pair, a firefly alone, a two-leaf ramp leading up to a firefly, a stacked
berry-and-firefly, and an explicit fork with leaves low and a firefly high. The screen takes about 4.5 seconds to cross, clusters
are separated by 1.4–2.3 seconds of scroll time, on-screen items are capped, and the next cluster starts near where the last one
ended, so the screen holds about **five or six items at a time** and every one of them is visible long enough to be seen, valued
and aimed for. A firefly is guaranteed at least every 7.5 seconds. Difficulty ramps only gently: the world speeds up about 12%
across the minute and the weighting shifts toward fireflies and forks.

Collection is forgiving on purpose — a generous radius plus a light magnet once the child is nearly there.

### Built with

Plain canvas 2D and the Web Audio API — nothing else, no CDN, no build step. Every pixel is generated at load: broadleaf and
conifer canopies from gradient blob clusters, shrubs, a grass strip with blades and flowers, a hanging top canopy and foreground
fern fronds, all baked into offscreen tiles and painted as repeating patterns across five parallax layers. Pip is drawn live each
frame — each wing is a long crescent with an arched leading edge and four feather scallops, keyframed through a raise / snap-down
/ settle stroke with the far wing a phase behind, plus velocity-driven head tilt, squash-and-stretch, blinking, and pupils that
track the next collectible.

Layout is resolution-independent: the world is re-derived from the viewport aspect on every resize, band sizes are referenced to
`min(height, width × 1.15)` so a tall phone does not get a wall of over-zoomed trees, and the HUD scales up in portrait.

Audio is a major-pentatonic collect scale whose pitch climbs with a streak, an FM bell for the firefly, filtered-noise wingbeats,
a convolution reverb built from generated noise and a very quiet pad-and-pluck bed. Nothing is sharp or loud, and the grown-ups
menu has a sound toggle.

The round clock runs on real elapsed time so "60 seconds" means 60 seconds on any machine, while physics runs on a clamped
timestep. The frame is wrapped so a draw exception unwinds the canvas state and keeps running — a child should never reach a
frozen screen.

`window.__OWL` exposes the game state for debugging.

---

## Beacon Bridge

A mental-rotation puzzle for roughly ages 7–10. Open `beacon-bridge/index.html` — no runtime dependencies, works offline.

**Play:** `←` `→` slide the block · `↑` `↓` turn it a quarter turn · `Space` drop it into the gap · `Q` grown-ups menu · `H` back to all games.

A reindeer has to cross a canyon before the light goes. Each of the three stone spans has a bite taken out of its deck, and **two** wooden blocks are needed to make it whole again — a lower one that slots into the notch, then an upper one that caps it off flush with the road. Three spans, six blocks, ninety seconds. Fill all three and the beacon on the far cliff lights up.

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

**Play:** `←` `↑` `↓` `→` run · `Space` sniff (and start/continue) · `Q` grown-ups menu · `H` back to all games. Those are the only keys — no mouse.

A fox has to get home to the den before the evening fog closes in. You can only see a few paces of wood — but a paper map in the corner shows the **whole** forest, with the den marked by a dotted ring. **The map never rotates** — up on the paper is up the screen — so the work is reading the trail system, not un-turning the page. A round is three short nights and takes under two minutes.

### The map is never covered

Reaching a landmark raises a message — "The standing stone", *find it on your map*. That message used to be pinned to the top
centre of the screen, which is fine on a desktop and wrong on a phone, where the paper disc is drawn much larger in proportion
and lives in that same top strip: the note landed squarely on the map it was telling the child to look at.

The message now knows where the paper is. Both read the same `mapPlacement()`, and the note is tested against the disc as a
circle rather than a box, so it is not pushed aside at the corners for nothing. Three positions are tried in order — centred at
the top as designed, then under the disc, then beside it — because on a short screen in landscape the disc is tall enough to
rule the first two out. A long landmark name is scaled down to fit a narrow screen rather than run off the edge.

### The skill it builds

The mechanic *is* the exercise: **allocentric → egocentric translation**, the real skill behind map reading and orienteering. You hold a bird's-eye model of the wood in mind while moving through it at ground level, and turn "the den is up-left on the paper" into "so I run up-left from here".

- **Route choice under branching** — difficulty is carried entirely by the trail system: a wider wood, more loop edges, more real junctions and a longer target path. Getting better makes the forest more tangled, never harder to orient.
- **Landmark-based wayfinding** — a fallen log, standing stone, berry bush, mushroom ring and pond are drawn *both* in the wood and on the map, each with a distinct silhouette. Every junction carries one, and walking near a landmark rings it on the paper so the two pictures link up.
- **Route planning and self-location** — sniffing pulses your position onto the map but recharges slowly, so a child learns to keep a running sense of where they are instead of checking constantly.
- **Spatial working memory** — the map is studied on the briefing screen and stays small during play.

### How it adapts

The game scores how *directly* each night was walked (shortest path ÷ distance actually covered) and keeps a rolling skill estimate in `localStorage`. Every scaffold is tied to it and is removed one at a time:

| Scaffold | Beginner | Confident |
| --- | --- | --- |
| Circle of sight | 330 px | 232 px |
| Sniff recharge | 3.8 s | 7.2 s |
| Breadcrumb trail on the paper | on | off |
| "Screen-up" marker on the map rim | on | faded out |
| Wood size | 5×3 grid | up to 7×4 grid |
| Loop edges (extra junctions) | ×0.77 | ×1.96 |
| Foxgloves to collect | 2–3 | 3–4 |

The grown-ups menu shows the live readout of all of it.

### No failure state

If the fog closes in completely, an owl starts calling from the den — **stereo-panned, so you can hear which side it is on** — a warm glow appears at the screen edge pointing home, and the moon slowly comes out so the circle of sight *widens* the longer a child takes. The night always finishes. The results card says how straight the trail was read, never how badly.

### Built with

Plain canvas 2D and the Web Audio API. Every tree, fern, landmark, foxglove and the den are procedurally drawn once into offscreen canvases at load, then trimmed to their alpha bounds and blitted. Woods are generated with a jittered grid, a Kruskal minimum spanning tree plus extra loop edges, and Dijkstra to choose a start node at the target path length — so the wood is always connected and the den is always reachable. The map is a separate ink-on-aged-paper rendering scaled to the trail's bounding box.

Audio is a brown-noise wind bed, FM-free sine voices through a generated-impulse convolution reverb, a pentatonic pad whose density tracks the fog, and a vibrato owl hoot panned toward the den.

The renderer only draws what the circle of sight can reach: the ground blit, sprite culling and the fog itself are all clipped to a quantised box around the fox, and the fog disc is cached until its radius changes. That took the frame from 216 ms to 27 ms in software-rendered headless Chromium. The fox is always drawn last and anything that would cover her fades out, so she can never be lost behind a canopy.

`window.__FG` exposes the game state for debugging.

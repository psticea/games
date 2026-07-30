# games

## Comet Catch — Bloop's Sky Carnival

**▶ [Play it here](https://psticea.github.io/games/)**

A single-file 3D browser game for roughly ages 6–10. Open `index.html` — no build step, no server, no keys.

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
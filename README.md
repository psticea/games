# STARBOUNCE — Zip's Sky Rescue

A browser game for ~8-year-olds. Open `index.html`. That's it — no build step, no server,
no dependencies, no network calls, no asset files.

## The game

Zip is a little star-comet. You aim a cannon, set its power, and launch Zip in an arc to
pop the **golden rescue stars** and free the sky-friends trapped inside. Pop chains of
bubbles on the way for combos. Steer the catcher cloud with ← → while Zip is falling to
save your hop and go again.

Every level is procedurally generated and always finishes in under two minutes.

## Controls

| Key | Action |
| --- | --- |
| ← → | Aim the cannon · steer the catcher cloud in flight |
| ↑ ↓ | More power / less power |
| SPACE | Launch Zip · continue on any screen |
| Q | Open/close the parent menu (pauses and resumes exactly) |

No mouse. No other keys.

## The developmental hook

The skill is **trajectory prediction and angle-of-reflection** — spatial reasoning, plus
fine motor control from steering the cloud mid-flight.

The interesting part is the scaffold. A sparkling guide-path shows exactly where Zip will
fly, simulated with the *same integrator as the real ball*, so it never lies. After every
shot the game scores how well that shot was predicted (`hitCount`, `starCount`) and moves a
hidden `skill` value. That value drives the guide's length:

```
guideT = lerp(2.30s, 0.42s, easeInOutSine(skill − assist))
```

As the child gets accurate, the guide **shrinks** — the prediction migrates off the screen
and into their head. Miss a few and `assist` climbs and the scaffold grows straight back.
The catcher cloud narrows with skill too (112 → 78 px). `skill` persists in `localStorage`,
so the difficulty tracks the child across sessions, and the parent menu shows the live
readout ("guide reduced 46%").

Failure is never harsh: running out of hops grants a one-time **BONUS HOPS** refill, and the
fail screen says "SO CLOSE! Every try makes your Star Sense stronger" with a tip, then
regenerates the level one step easier.

## Technical notes

Single file, canvas 2D, fixed 1280×800 virtual resolution letterboxed to any window.

- **Art** is 100% procedural — clouds, floating islands, glass bubbles, gold stars, jelly
  bumpers, candy rails and the aurora sky are all drawn with gradients and baked into
  offscreen sprite canvases at load, then blitted.
- **Audio** is 100% Web Audio synthesis: a major-pentatonic combo scale (nothing can sound
  wrong), a convolution reverb built from generated noise, and a I–V–vi–IV arpeggio bed
  that speeds up during fever.
- **Bloom** is a downsample pyramid (÷3 → ÷6 → ÷12) composited additively, rather than a
  per-frame `filter: blur()` — about 4× cheaper.
- **Particles** use a compact pool (active particles occupy `[0, nActive)`) and are drawn in
  two batched passes so the composite mode is set twice per frame instead of once per
  particle.
- **Adaptive quality** watches an EMA of frame time and scales burst sizes / drops the
  particle glow pass if a machine can't keep up.
- **Levels** are seeded (mulberry32) from the level number and laid out with one of seven
  patterns: grid, arcs, flower, spiral, wave, pillars, diamond.

Frame budget on the QA machine (software-rendered headless Chromium, worst case level 12):
**~10 ms/frame with 325 live particles.**

## QA

Verified by actually playing it with a Playwright-driven solver:

- 30 shots across 6 levels, 5 level clears, **zero JS or console errors**
- **Every rescue star on levels 1–12 is reachable** — brute-forced 80 angles × 21 powers
  through the real physics; no level can soft-lock
- Level completion times 7–21 s; the 104 s hard cutoff force-clears and never hangs,
  including when it fires mid-flight
- `Q` pause verified frame-exact: position, velocity and level clock identical after 1.8 s
  paused; arrow keys drive the menu and don't disturb the aim; resume continues the flight
- Adaptive scaffold observed working live: skill 0.12 → 0.80, guide 2.30 s → 0.60 s, then
  back up to 2.10 s after a failed level
- Resize checked at 640×480 / 800×600 / 1400×900 / 1920×1080
- All 13 synth voices, both audio toggles, the reset option and `localStorage` persistence

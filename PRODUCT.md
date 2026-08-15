# Product

<!-- impeccable:product-schema 1 -->

## Platform

web

## Users

Children of roughly 5–10, playing on a grown-up's phone in a browser or from a home-screen icon, usually handed the phone for a few minutes (waiting rooms, car journeys, the end of a day). A grown-up is nearby: they open the site, hand it over, and sometimes step into a per-game "grown-ups menu" to change sound or round length. Some play on a desktop with arrow keys.

*(Audience age and usage scene inferred from the games' copy, reading level, and controls; confirmed by the user's brief that the home page must appeal to "kids and adults".)*

## Product Purpose

Four small, complete browser games, each a single round of 60–90 seconds, each practising one thinking skill. Success is a child choosing a game unaided and playing it straight away; and a grown-up feeling the whole thing is safe and unhurried.

## Positioning

Every game trains one named cognitive skill inside a very short round with **no way to lose**: Starfall Meadow — trajectory prediction; Owlet Grove — timing; Beacon Bridge — mental rotation; Foxglove Trail — map reading. No accounts, no ads, no purchases, no streaks, no score-chasing loops.

## Operating Context

Opened from a phone browser or added to the home screen, then played full screen in one hand. The whole site is: pick a game → play a round → go home or open the grown-ups menu. Each game asks for full screen on the first tap; Starfall Meadow also asks for landscape. Phones are the primary device; a desktop keyboard path exists and must keep working.

## Capabilities and Constraints

- Static site on GitHub Pages. No build step, no framework, no bundler, no dependencies.
- Each game is one self-contained HTML file with inline CSS and JS, rendering to `<canvas>`.
- The home page is a plain `index.html` with inline SVG art for each game.
- Fonts come from Google Fonts (Baloo 2, Nunito); network is otherwise unused.
- Persistence is `localStorage` only (best score, sound, settings).
- **The home page must fit one screen with no scrolling, as four sections for the four games** (confirmed by the user).
- Tap targets are finger-sized; a home button and a grown-ups button are visible in every game.

## Brand Commitments

The four game names and their illustrated cast (Pip the fox cub, the owlet, the fox and the beacon, the trail fox) are fixed, as is the existing in-game art of each. The voice is warm, plain and unhurried, and speaks to a child without baby-talk.

## Evidence on Hand

All four games exist and are playable in this repository. Each has bespoke inline SVG key art on the home page and a full canvas-drawn world inside. There are no testimonials, download counts, reviews, or usage numbers, and none may be invented.

## Product Principles

1. **A child must be able to start without reading.** Pictures and characters carry the choice; words confirm it.
2. **No losing, no punishment, no pressure.** Rounds end kindly.
3. **A grown-up stays in control.** Settings and the way out are always one tap away.
4. **Nothing extractive.** No accounts, no ads, no purchases, no streak mechanics.
5. **One screen, one hand, one tap to play.**

## Accessibility & Inclusion

Pre- and early-readers must be able to navigate by picture alone. Tap targets are at least 44px. Motion respects `prefers-reduced-motion`. The desktop keyboard path (arrow keys, Enter, Q, H) must keep working.

# Tile Bloom — DESIGN.md

> Single source of truth for the look & feel of **Tile Bloom**, a relaxing triple-tile
> matching puzzle. Version **1.5.1**. This document follows the open DESIGN.md standard
> and is ready to paste into Google Stitch ("Start with your design" → *Paste existing
> DESIGN.md*).

## 1. Overview

Tile Bloom is a calm, premium "collect-three" tile-matching puzzle for iPhone (and web).
The player taps glassy tiles stacked on a board; identical tiles fly into a 7-slot tray
below and bloom away in a neon burst when three match. 120 procedurally-generated levels
across four themed worlds (Coral Reef → Sweet Bakery → Starry Orbit → Zen Garden).

Vibe keywords: **glassy · neon-glow · deep-sea calm · juicy feedback · cozy mascot ·
premium casual**.

Design goals:

1. Instant readability — every tile is legible in ~0.4s at arm's length.
2. Calm but alive — backgrounds drift slowly; nothing flickers or rushes.
3. Juicy, weighty feedback — every tap answers with arc, squash, glow, chime, haptic.
4. Original trade dress — glass-and-neon tiles on deep gradient worlds with a flower
   mascot (Bloomie). Deliberately distinct from flat pastel tile games.

## 2. Brand

- Name: **Tile Bloom** (two words, title case). Tagline: *Triple Match Puzzle*.
- Logo: wordmark in Baloo 2 800, gradient from ice-white to accent cyan, second word in
  accent color.
- Mascot: **Bloomie** — a round white face with 5 accent petals, blush, sprout leaf.
  Lives in a glass bubble. Moods: idle (bob), happy (petals wiggle), sad (droop, frown).
- App icon: three overlapping glass tiles (cyan heart, gold star, pink flower hero)
  on ocean-blue gradient with light rays. No text.

## 3. Color

### 3.1 Core (brand chapter = Coral Reef)

| Token | Value | Usage |
| --- | --- | --- |
| `bg.top` | `#0d4a6e` | top of any gradient backdrop |
| `bg.mid` | `#072c47` | middle stop |
| `bg.bot` | `#041420` | bottom stop / status-bar tint |
| `accent` | `#59e2ff` | primary interactive glow, neon frames |
| `glass` | `#8ae2ff` | tile fill tint at 30% / 12% alpha |
| `ink` | `#eaf7ff` | primary text |
| `ink.dim` | `rgba(234,247,255,0.62)` | secondary text |
| `gold` | `#ffd166` | coins, stars, "reward" surfaces |
| `danger` | `#ff5a5a` | fail pulse |

### 3.2 Chapter palettes (backgrounds + accents swap, glass style constant)

| Chapter | bg gradient (top→bot) | accent | ambient |
| --- | --- | --- | --- |
| Coral Reef (1–30) | `#0d4a6e → #041420` | `#59e2ff` | rising bubbles + light rays |
| Sweet Bakery (31–60) | `#6b4226 → #2a1a10` | `#ffb0c8` | drifting crumbs, steam |
| Starry Orbit (61–90) | `#2b1a5e → #0b0620` | `#b18cff` | twinkling stars, aurora |
| Zen Garden (91–120) | `#2c5540 → #0d1f16` | `#8fe6a4` | falling petals, fireflies |

Icon color pools per chapter keep 10 vivid hues each (`#7ff3ff, #ffd166, #ff8fa3, #9dffd0,
#c9a7ff, #ffa94d, #8ad7ff, #ffe3a3, #ff6f91, #a0f0ff` for Coral Reef).

Rules: accent is used ONLY for interactive/glow (frames, primary buttons, active
toggles). Body text is always ink/ink-dim, never accent. Gold is reserved for economy
(coins/stars/reward buttons). Never place gold next to danger.

## 4. Typography

- Display: **Baloo 2** (600–800). Buttons, headings, HUD numbers, logo.
- Body: **Nunito** (400–800). Paragraphs, labels, toasts.
- Both are OFL-licensed via @fontsource, bundled offline (no Google Fonts CDN).

Scale (px @ 390 width):

| Role | Font | Size | Weight |
| --- | --- | --- | --- |
| Logo | Baloo 2 | 52 | 800 (gradient fill) |
| Modal title | Baloo 2 | 25 | 800 |
| Primary button | Baloo 2 | 17–21 | 700 |
| HUD number | Baloo 2 | 14 (chip) | 800 |
| Body / steps | Nunito | 14–15 | 600 |
| Micro label | Nunito | 11–12 | 800, uppercase, +0.42em tracking |

## 5. Layout & spacing

- Base grid: full-bleed canvas; UI overlays anchored to safe areas
  (`env(safe-area-inset-*)`), iPhone notch & home-indicator aware.
- HUD top bar: 54px tall zone + 8px below safe-top. Pause button (46×46) left, chips
  centered with 10px gaps.
- Board zone: centered between HUD and tray; tiles sized 30–84px cell (0.92 fill ratio)
  computed per level; stacks offset +0.2 cell per layer to the upper right.
- Tray: 7 slots, slot = 0.74× cell (width-budget clamped), 5px gaps, 16px radius glass
  panel floating 26px above the booster bar.
- Booster bar: 3 buttons (92px wide, 20px radius glass) pinned bottom + 10px safe-bottom.
- Radii: tiles 22% of size · buttons 18 (big 22) · cards 30 · chips pill.
- Spacing rhythm: 4/8/10/14/26. Cards padding 26×24.

## 6. Components

**Glass tile** (hero component): rounded rect 22% radius; vertical glass gradient
(accent-glass 30% → 12% alpha); top gloss streak (white 32% → 0, upper half); baked drop
shadow (55% black, 7px y-offset); neon inner frame 4.5% of size at 95% accent alpha;
outer faint edge 35%; corner spark dot. Blocked (buried) tiles: same geometry at 10%
fill, 22% frame, icon 62% alpha + dark veil 30%. Top-of-stack tiles pulse a faint accent
outline (alpha 0.15–0.28 sine).

**Tray slot**: rounded well, white 5.5% fill, 14% inner stroke; tiles settle at 0.74×
scale with spring-lerp regrouping (identical icons cluster).

**Buttons**: primary = accent gradient pill, dark ink text `#04222e`, 28px glow shadow;
ghost = 7% white glass + 16% border; reward = gold gradient `#ffe3a3→#ffb84d`, brown ink.
Press = 0.95 scale (120ms). Disabled = 45% opacity.

**HUD chips**: 10px-blur glass pills, 800-weight numerals, uppercase micro labels.

**Heart bar (v1.3)**: dark pill (5 pips, 22px each) under the chapter chip; filled pip =
rose gradient `#ffa3b5→#f4517a→#bf2550` + ink outline + highlight ellipse; empty pip =
light desaturated silhouette (grayscale 1.15 brightness, 32% alpha). Timer text 12px
800-weight under the pill; +1 pip pops 0.4s overshoot 1.4.

**Mission row (v1.3)**: glass card row — 38px icon tile (per kind: trio/flag/bolt/
star/sparkle), 800-weight title, 9px progress bar (cyan→accent fill, green when done),
progress `a / b`, reward chips (heart + coin minis) and a compact Claim button.

**Gift card (v1.3)**: gold-tinted glass row — 44px gift icon tile, copy + reward line,
compact claim button; claimed state = 55% opacity.

**Modals**: full-screen 50% black scrim + 7px blur; card = `rgba(10,24,36,0.86)`, accent
45% 1px border, 30px radius, pop-in (0.32s, overshoot 1.3). Star reveal staggered 260ms.

**Toast**: bottom glass pill, fade-rise 1.7s.

**Ad surface (mock)**: black 88% overlay, "TEST AD" gold chip (blink), animated creative
box, progress bar, claim button pulses when ready.

**Bloomie mascot**: 192px on home; bubble = radial glass; 5 petals rotate ±72°; face
white circle; eyes `#14303c` + glints; smile bezier; blush 42% pink; sprout leaf pair.
Sad = frown path + 55% petal opacity.

## 7. Motion

| Event | Motion |
| --- | --- |
| Tile pick | 300ms quadratic-bezier arc (apex −80px), scale 1→1.2→1, ±0.16rad wobble |
| Tile lands | spring lerp to grouped slot (14/s) |
| Match | neon ring expand 14→60px, 12 shards (420px/s gravity), `+score` popup rise 46px/s |
| Combo | ×N gold pill swaps into the score-chip label slot, pop 0.35s overshoot 1.4 per combo step (never floats over the board); pitch shift per combo |
| Level complete | white flash 0.55 → 110 confetti 1.8–3.2s, staggered stars |
| Fail | 0.55s screen shake ±9px; tray red pulse 1.2s |
| Board idle | accent outline sine 2.4rad/s; ambient particles drift 8–26px/s |
| Reduced motion | all durations → ~0 (prefers-reduced-motion) |

Sound (all synthesized, WebAudio): pick pluck 560→720Hz; match pentatonic arpeggio
(+combo semitone); fail saw descent; win fanfare; coin ding; generative ambient music
84bpm pentatonic random-walk, root per chapter (A/C/D/G), detuned-triangle pads.

## 8. Screens

1. **Home** — logo, subtitle, Bloomie (192), chapter·level chip, **heart bar (5 pips
   + regen countdown)**, primary Play, ghost How-to + Missions (with red claim badge) +
   gear, star/coin stats, version footer; decorative glass tiles float in corners (50%
   alpha).
2. **Tutorial (modal)** — 3 steps with tile trio visual + copy.
3. **Gameplay** — HUD (pause, level chip, score chip with inline ×N combo pill,
   coins, **hearts chip**), board arena (glass plate hugging tile bounds), 7-slot
   tray, booster bar, intro banner pill (level · chapter, slide 1.05s).
4. **Pause** — resume / restart / quit.
5. **Level complete** — 3 star slots (gold pop), score/best grid, +coins, 2× coins
   (rewarded), next/replay/home; chapter-end variant headline.
6. **Fail** — sad Bloomie, heart-cost hint line (rose, heart mini icon), continue
   (rewarded, 8s countdown ring), retry, home.
7. **Settings** — sound/music/haptics switches, EN/TR segmented, about block (version,
   copyright, privacy link).
8. **Daily missions (modal)** — gold gift card on top, 3 mission rows (progress bars,
   claim buttons, heart+coin rewards), Later.
9. **Out-of-hearts gate (modal)** — 5 empty heart sockets, "next heart in mm:ss"
   countdown, rewarded-ad +1 heart button, go-to-missions, later.

Flows: Home → Play → (intro 1s) → Gameplay → Complete → [Next → maybe interstitial] →
next level; Fail → Continue (rewarded, ≤2/level, rescues the heart toll) / Retry
(−1 heart) / Home (−1 heart); hearts empty → gate → [rewarded ad | missions | wait].

Gameplay economy (v1.3): 5 hearts max, one lost per failed level, 20min/heart regen;
daily missions + gift are the heart faucet. Difficulty curve: icon variety 5→12,
board budget 36→150 tiles, layers 2→5 — every level still provably clearable, but the
careless-play death rate rises from ~15% (lvl 1) to ~85% (lvl 12) to ~100% (lvl 30+).

## 9. Accessibility & platform

- Contrast: ink on any bg ≥ 7:1; accent used for emphasis only.
- Hit targets ≥ 44px; single-tap interaction, no drag/swipe required.
- `prefers-reduced-motion` respected; haptics optional; text not below 11px.
- iPhone-first: safe-area insets, 2× DPR cap, offline-capable (no network calls in web
  build; ad SDK placeholder only in the iOS shell).

## 10. Assets

| Asset | File |
| --- | --- |
| App icon (master) | `assets/app-icon.svg` (+1024/512/192/180 PNG) |
| Design tokens | `design-tokens.json` |
| Screen mockups | `screens/menu.svg`, `screens/gameplay.svg`, `screens/level-complete.svg`, `screens/fail.svg` (390×844) |
| Icon glyph library | 45 procedural canvas glyphs (fish, shell, starfish, crab, octo, bubble, pearl, anchor, wave, croissant, cupcake, cookie, donut, candy, coffee, icecream, cherry, macaron, rocket, planet, comet, ufo, alien, galaxy, spark, butterfly, bee, clover, mushroom, acorn, bird, pebble, lotus, candle + 11 universal) — drawn in code, no binary assets |

No third-party imagery, fonts, or audio; everything is original and generated.

© 2026 Elchin Jabbarli — Tile Bloom. Version 1.0.1.

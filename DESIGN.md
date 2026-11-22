# Tile Bloom — DESIGN.md

> Single source of truth for the look & feel of **Tile Bloom**, a relaxing triple-tile
> matching puzzle. Version **1.7.1**. This document follows the open DESIGN.md standard
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

**Frost-sealed tile (v1.6.0 special)**: top-of-stack tile sealed under ice — whitish
frost veil (~62% icy white `rgba(214,240,255,…)`) with crack strokes that grow per
remaining layer; unpickable (glassy knuckle-tap sfx on tap). From level 8, 2 layers
(3 from level 60), 4–16 surface tiles per level; every trio (or hammer
smash) thaws ALL frozen tiles by one layer — shedding the last layer
shatters white-blue particles.

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
| Combo | slim flame chip (amber `#ffd166`→coral when ≥5) appears between the level and score chips while the combo window runs, `×N` pops 0.35s overshoot 1.4 per step; pitch shift per combo |
| Combo fanfare (v1.7.0) | milestones ×3/×5/×8/×12: 3 firework volleys around the board + big praise banner ("Nice!/Great!/Amazing!/Unstoppable!" scale-in over the board, 1.35s); ×8+ adds a 40-confetti shower |
| Frost thaw (v1.6.0) | per-tile white-blue shard burst (9 shards, 0.55s) each time a layer sheds |
| Hammer smash (v1.7.0) | armed button pulses (tilted head + orange glow); smash = 18 shards of the victim's own color + frost shatter if icy + 0.4 screen shake + "SMASH!" popup |
| Wand zap (v1.6.0) | gold + violet sparkle volleys on the trio tiles, `+pts` popup, 0.16 flash |
| Booster press | scale 0.93 0.12s; disabled → saturate 0.55 |
| Level complete | white flash 0.55 → 110 confetti 1.8–3.2s, staggered stars |
| Fail | 0.55s screen shake ±9px; tray red pulse 1.2s; in-flight picks fly back to the board; rewarded ad is a CHOOSER (continue/+1 heart/+100 coins), no countdown pressure (v1.8.0) |
| Board idle | accent outline sine 2.4rad/s; ambient particles drift 8–26px/s |
| Particle layering (v1.7.0) | ambient drift renders behind the tiles; every celebratory FX (coins, bursts, banners, popups) renders in a second pass ABOVE the tiles so nothing hides mid-board |
| Reduced motion | all durations → ~0 (prefers-reduced-motion) |

Sound (all synthesized, WebAudio): pick pluck 560→720Hz; match pentatonic arpeggio
(+combo semitone); fail saw descent; win fanfare; coin ding; frozen glassy knuckle tap
1400Hz sine + noise; thaw double snap + melt drip; hammer smash woody knock 190Hz triangle + two noise crunches; wand zap three
rising stars + sweep; generative ambient music 84bpm pentatonic random-walk, root per
chapter (A/C/D/G), detuned-triangle pads.

## 8. Screens

1. **Home** — logo, subtitle, Bloomie (192), chapter·level chip, **heart bar (5 pips
   + regen countdown)**, primary Play, ghost How-to + Missions (with red claim badge) +
   gear, star/coin stats, version footer; decorative glass tiles float in corners (50%
   alpha).
2. **Tutorial (modal)** — 5 steps with tile trio visuals + copy: collect-to-tray,
   trio burst, tray overflow = fail, frost sealing (v1.6.0), the Hammer (v1.7.0).
2b. **Journey map (v1.8.0)** — the chapter chip on Home opens the map: 120 level
   nodes on a winding dashed path through all four chapters (30 per section,
   header counts cleared levels); done nodes mint with star pips, the current
   node pops in with an accent pulse ring, locked nodes show a lock glyph and
   refuse politely; every 10th level is a bigger milestone tile, chapter ends
   (30/60/90/120) are gold trophy gates. Tapping any unlocked node starts that
   level through the hearts gate.
2c. **Tile album (v1.8.0)** — 100 collectible icons across the four chapter
   tabs (baked glyph canvases; uncollected render as dark silhouettes with a
   grayscale filter, collected show a tile count). Distinct-icon milestones
   10/25/50/75/100 pay one-time coins (25/50/75/100/200) with a toast.
   Collection is monotonic — undo never un-collects.
3. **Gameplay** — HUD (pause, level chip, slim flame combo chip while a combo runs
   (v1.7.0), score chip, coins, **hearts chip**), board arena (glass plate hugging tile
   bounds; deep stacked towers with right/up layer drift, headroom reserved above the
   grid), 7-slot **staged glass tray** (raised plate, drop shadow, recessed wells;
   picked tiles seat at 0.86 slot scale so the well lip stays visible), **compact candy
   booster bar** (~54px tall, icon+label head row + cost pill, color-coded mint/violet/
   amber/gold-wand/ember-hammer — Undo · Shuffle · Hint · Wand (v1.6.0: zaps one
   visible trio straight off the board) · **Hammer (v1.7.0: arm it, then smash any
   top tile straight off the board — even frost-sealed)**, FREE/coin pills candy-pop
   with dark ink; armed Hammer pulses with an ember glow), intro banner pill (level ·
   chapter, slide 1.05s); tray and booster row separated by ~24px breathing gap.
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
daily missions + gift are the heart faucet. Difficulty curve v5 (v1.6.0 "much harder,
more icons, more fun" brief): icon variety 6→24 by level 21 (100-glyph library, chapter
pools of 35–37), board budget 36→216 tiles (72 trios),
layers 2→8 with density-capped cells for real deep towers, greedy pressure target
3→6 by level 25, and the frost special — 2–3-layer surface ice
(lvl 8+) — every level still provably clearable, but the careless-play
death rate is 100% from level 8 (only levels 1–2 stay gentle as the fair start).
v1.7.0 "the fun pass": the Hammer booster smashes
any tile (frost included) for tray-pressure rescue, and combo milestones (×3/×5/×8/
×12) fire board-wide fireworks with localized praise banners — undo now also
re-freezes thawed frost honestly, and a fail returns in-flight picks to the board.
v1.7.1: coin bags removed per player feedback. v1.7.2: the golden bloom free-pop
special removed too — every tile now rides the same tray economy (board budget
219→216, no free pops anywhere).
v1.8.0 — the retention pass (no new content systems for the board itself):
levels 1–10 are the heart-mercy band (failures cost NO heart, the fail screen
says so), two consecutive fails on a level auto-show a free 4.5s hint, the
level-complete screen gains a run report (best combo ×N, boosters used, PERFECT
RUN badge for clean clears, NEW BEST! badge when beating the stored best), the
fail screen's rewarded ad became a CHOICE (continue this level / +1 heart /
+100 coins — the 8s countdown pressure is gone), daily missions widened to ten
kinds (thaw layers, booster uses, hammer smashes, album collects, single-level
score) and roll adaptively to the player's level, the booster bar grew a "?"
info modal (what each booster does + cost + first-use-free tip), and the tile
album + journey map give the long-term goal the game lacked.

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
| Icon glyph library | 100 procedural canvas glyphs (four chapter pools of 35–37: fish, shell, starfish, crab, octo, bubble, pearl, anchor, wave, seahorse, jellyfish, turtle, squid, coral, puffer, shrimp, urchin, manta, sailboat, lighthouse, seagull, croissant, cupcake, cookie, donut, candy, coffee, icecream, cherry, macaron, waffle, pretzel, pie, teapot, milkshake, gingerbread, pancake, lollipop, popcorn, icepop, brownie, cheesecake, rocket, planet, comet, ufo, alien, galaxy, spark, astronaut, satellite, telescope, meteor, atom, radar, robot, capsule, beacon, blackhole, wormhole, moonrock, butterfly, bee, clover, mushroom, acorn, bird, pebble, lotus, candle, dragonfly, ladybug, snail, pinecone, bamboo, bonsai, owl, fox, koi, hedgehog, lantern, windchime + 18 universal incl. balloon & pinwheel) — drawn in code, no binary assets |

No third-party imagery, fonts, or audio; everything is original and generated.

© 2026 Elchin Jabbarli — Tile Bloom. Version 1.0.1.

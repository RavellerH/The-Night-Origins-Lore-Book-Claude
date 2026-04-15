# Deepborn Idle Game — Engine Decision & Development Plan

> **Last updated:** April 13, 2026
> **Status:** Planning phase — engine decision pending

---

## Current State Summary

| Asset | Status |
|---|---|
| **Game Design Document** (`idlegameplan.md`) | ✅ Complete — 574 lines, full Idle RPG + VN spec |
| **Brainstorm/Research** (`perplexity brainstorm.md`) | ✅ Complete — engine comparison, monetization, launch strategy |
| **Godot Project** (`deepborn-idle-game/`) | ⚠️ Barely started — Godot 4.6 scaffold, 0 scenes, 0 scripts, 0 assets |
| **Existing Lore Navigator** (`index.html`, `world-map.html`, `plot-diagram.html`, `quiz.html`) | ✅ Working — self-contained HTML/JS games already deployed |
| **Lore Content** (`../Lore/`) | ✅ Thousands of pages — source of truth |
| **Art Pipeline** (`ART_PROMPTS.md`, Concept Art folders) | ✅ Established |

---

## The Core Question: Godot vs. Full HTML/JS

### Recommendation: **Build as HTML/JS (Plain Web App)**

Here's why:

### 1. You Already Have the Infrastructure
- `index.html` — A fully functional single-file web app with lore rendering
- `world-map.html` — Interactive SVG map with JavaScript
- `plot-diagram.html` — Canvas-based visualization
- `quiz.html` — Interactive quiz with scoring
- All four work by **double-clicking the file** — no server, no build step

Adding a fifth (`idle.html` or `game/`) costs nothing new. You already know the pattern.

### 2. Your Team Knows JavaScript
- Your entire lore navigator is vanilla JS + HTML + CSS
- Your AI coding agents (Claude, Gemini, Codex) are strongest at JS/HTML
- GDScript requires learning a new engine UI, node system, signal architecture
- Godot's editor is a productivity tax for teams building complex 3D games — for a 2D idle RPG it's mostly overhead

### 3. Idle RPG + VN is Perfectly Served by Web
Your game is **numbers, UI cards, timers, and dialogue boxes**. These are HTML's strongest primitives:
- Hero cards = `<div>` elements with CSS
- Dialogue boxes = CSS positioned overlays with JS state machine
- Timers = `setInterval()` / `requestAnimationFrame()`
- Save/load = `localStorage` (works everywhere, no config needed)
- Animations = CSS transitions + keyframes (or Canvas for damage numbers)

Godot's advantage is rendering, physics, audio mixing, and scene management. You need almost none of that.

### 4. Distribution is Identical
| Platform | Godot | HTML/JS |
|---|---|---|
| Browser | HTML5 export (heavy WASM, 30-80MB load) | Native (light, instant) |
| Android | AAB via Play Console | PWA (Progressive Web App) — installable, no Play Store gate |
| iOS | Requires Xcode + $99/yr Apple Developer | PWA — works in Safari, no approval |
| Desktop | Requires export per OS | Works everywhere — it's a webpage |
| Itch.io | Upload WebGL build | Upload HTML file — works immediately |

For a narrative-driven idle game, **instant load matters more than rendering fidelity**. An HTML file loads in milliseconds; a Godot HTML5 export loads a 30-80MB WASM bundle.

### 5. Your Game Doesn't Need What Godot Excels At
| Godot Strength | Your Game Needs It? |
|---|---|
| 2D sprite rendering & animation | ❌ Your game is UI cards and text |
| Physics (Jolt) | ❌ Your game has no physics |
| Audio mixing & spatial sound | Maybe — HTML5 `<audio>` handles it |
| Scene tree & node management | Overkill for tab-based UI |
| Resource pipeline (.import) | Overkill for a game with static assets |
| Export pipeline complexity | Unnecessary — single HTML file |

### 6. AI Code Agents Are Better at JS Than GDScript
From your own research notes:
> "If Claude keeps making GDScript syntax errors, switching to C# gives Claude much better accuracy"

Translation: **JS is the native language of AI code generation**. Claude, Gemini, and Codex can write production-quality vanilla JS with zero configuration. GDScript requires constant documentation lookups.

### 7. Precedent: Your Existing Games Work
`quiz.html` already proves you can build interactive JS games. The lore navigator itself is effectively a single-page application. Adding game mechanics (timers, DPS counters, prestige system) is an extension of what already works.

---

## When Godot *Would* Make Sense

Godot is the right choice **if and only if** you plan to:
- Port to **console platforms** (Switch, PlayStation) — HTML/JS can't do this
- Build a **fully animated 2D game** with sprite sheets, parallax, particle systems
- Need **complex audio layering** (dynamic music that responds to game state)
- Want to ship as a **native mobile app** on Play Store / App Store (not PWA)
- Plan to sell it as a **premium standalone product** (Steam, itch.io desktop)

For a narrative idle RPG that starts as a **lore companion game**, none of these apply yet.

---

## The Phased Approach

### Phase 1: HTML/JS Prototype (Recommended First Step)
Build the core idle loop in a single `game.html` file matching your existing pattern:
- Tap-to-damage mechanic
- Hero card DPS system
- Stage progression with bosses every 25 stages
- Gold economy and upgrade buttons
- Offline progress (localStorage timestamps)
- Prestige system
- Visual novel scenes triggered at milestone stages (reuse your existing VN pipeline)

**Timeline to playable:** 1-2 weeks with AI assistance
**File count:** 1-3 files (HTML + CSS + JS)
**Distribution:** Double-click to play, or host on GitHub Pages / itch.io

### Phase 2: Polish & Expand
- Add damage number animations (Canvas or DOM)
- Sound effects (HTML5 `<audio>`)
- Save/load system with multiple slots
- Character portraits (your existing art pipeline)
- VN portrait system with dialogue boxes
- Faction trust web UI
- Zone corruption map (SVG like world-map.html)

### Phase 3: Mobile PWA
- Add `manifest.json` and service worker
- Installable on Android/iOS as a "native-like" app
- Works offline after first load
- No app store approval needed

### Phase 4: Godot Port (Only If Needed)
If the game gains traction and you want:
- Native mobile app store presence
- More complex animations/cutscenes
- Desktop standalone distribution

Then port to Godot with a clear spec, proven game loop, and actual player data. **Porting a proven game is easier than building unproven in a heavier engine.**

---

## Architecture Sketch (HTML/JS)

```
game.html
├── <style> — all CSS (cards, animations, layouts, VN dialogue boxes)
├── <body> — game UI skeleton (tap area, hero roster, resource bars, VN overlay)
└── <script>
    ├── GAME_STATE = { ... } — all mutable state
    ├── HEROES = [ ... ] — hero definitions (from idlegameplan.md roster)
    ├── FACTIONS = { ... } — faction data
    ├── STORY_SCENES = [ ... ] — VN scene triggers and content
    ├── 
    ├── Core Loop:
    │   ├── tick() — main game loop (60fps via requestAnimationFrame)
    │   ├── calculateDPS() — hero passive damage
    │   ├── tapDamage() — Commander's personal attack
    │   ├── advanceStage() — stage counter, boss checks
    │   ├── earnGold() — gold from kills
    │   └── saveGame() — localStorage persistence
    │
    ├── Economy:
    │   ├── upgradeCommander()
    │   ├── hireHero()
    │   ├── upgradeHero()
    │   └── buyBuilding()
    │
    ├── Systems:
    │   ├── bossTimer() — 30-second boss fight countdown
    │   ├── offlineProgress() — calculate AFK earnings on load
    │   ├── prestige() — reset + permanent buffs
    │   ├── corruptionSystem() — zone corruption rates
    │   └── factionTrust() — trust pair calculations
    │
    ├── Visual Novel:
    │   ├── triggerScene(sceneId) — pause game, show VN overlay
    │   ├── renderDialogue(scene) — display text, portraits, choices
    │   └── applyDecision(choice) — shift stats based on player choice
    │
    └── UI:
        ├── renderHeroes() — update hero card display
        ├── renderResources() — gold, shards, essence display
        ├── renderZones() — corruption map
        └── renderVN() — dialogue box, portrait display
```

---

## File Structure (Recommended)

```
Idle/
├── game.html              # Main game file (self-contained, like index.html)
├── game.css               # Optional: extracted styles if file gets too large
├── game.js                # Optional: extracted logic if file gets too large
├── scenes/                # VN scene data (JSON)
│   ├── act1.json
│   ├── act2.json
│   ├── act3.json
│   └── act4.json
├── heroes/                # Hero definitions (JSON)
│   ├── light-mages.json
│   ├── nature-mages.json
│   ├── druids.json
│   ├── dark-mages.json
│   └── fire-mages.json
├── portraits/             # Character portrait images
│   ├── commander-silhouette.png
│   ├── avery.png
│   ├── nate.png
│   └── ...
└── audio/                 # Sound effects and music
    ├── tap.ogg
    ├── boss-warning.ogg
    └── vn-transition.ogg
```

**Or keep it single-file** (matching your existing pattern):
```
Idle/game.html  # Everything in one file, like index.html
```

---

## What to Do With the Godot Project

**Don't delete it.** Keep it as an option:
- It costs nothing to maintain an empty Godot scaffold
- If Phase 1 prototype proves fun, the Godot project becomes the Phase 4 target
- The `.gitignore` and `.gitattributes` are already set up

But don't invest time in it until you have a **proven game loop** in HTML/JS first.

---

## Decision Matrix

| Factor | HTML/JS | Godot | Winner |
|---|---|---|---|
| Development speed | Fast (your stack) | Slow (new engine) | HTML/JS |
| AI code quality | Excellent | Good with help | HTML/JS |
| Time to playable | Days | Weeks | HTML/JS |
| Browser performance | Instant load | 30-80MB WASM | HTML/JS |
| Mobile PWA support | Native | Requires export | HTML/JS |
| Native app stores | Not directly | Full support | Godot |
| Console ports | Impossible | Possible | Godot |
| 2D animation quality | CSS/Canvas (good) | Built-in (great) | Godot |
| Learning curve | Zero (you know JS) | Medium (new UI) | HTML/JS |
| Risk if game fails | Low (HTML file) | Medium (abandoned project) | HTML/JS |
| **Overall for Phase 1** | ✅ | ⚠️ | **HTML/JS** |

---

## Next Steps

1. **Confirm this direction** — Agree on HTML/JS for the prototype
2. **Start building** — Begin with the core tap + DPS + gold loop in `game.html`
3. **Iterate** — Add heroes, bosses, offline progress
4. **Test** — Play the prototype, validate the fun factor
5. **Decide on Godot** — Only if the game proves out the HTML/JS prototype

---

## References

- Game Design Document: `idlegameplan.md` (574 lines, complete spec)
- Brainstorm/Research: `perplexity brainstorm.md`
- Existing Lore Navigator: `../index.html`
- Godot scaffold: `deepborn-idle-game/` (empty, on standby)

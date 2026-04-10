# Lore Navigator — Games Format Guide

> This file documents the interactive game format used in `Games/`.
> Distinct from the main lore webapp (`index.html`) — games are standalone story experiences, not reference tools.
> Cross-reference: [[plan]] (main webapp upgrades) | [[agents]] (lore sync protocol)

---

## What the Games Are

The games are **scene-based interactive story walks** — the player moves through a moment in the story from a character's point of view. Not a quiz, not a reference browser. A feeling.

Each game is:
- A single emotional beat, not a chapter summary
- First-person character POV (Avery, Callen, Fran, Isaac, etc.)
- Linear progression with optional hotspot exploration
- Self-contained — no dependency on the main webapp
- Atmospheric — the visual design carries as much weight as the text

---

## Existing Games

| File | Character | Moment |
|------|-----------|--------|
| `Games/vleyrra/index.html` | Avery | First night in Vleyrra Forest, meeting Nate |
| `Games/blue-river/index.html` | — | Blue River Village |
| `Games/bronzefinger/index.html` | — | — |
| `Games/walking-sim/index.html` | — | — |

---

## File Structure (per game)

```
Games/
  [game-name]/
    index.html        ← single self-contained file, all HTML/CSS/JS
```

Background images live in `/game-bg/` at the root Lore Navigator level and are referenced with `../../game-bg/filename.jpeg`.

---

## HTML/CSS Architecture

### Fonts (always the same)
```css
@import url('https://fonts.googleapis.com/css2?family=Cinzel:wght@400;600&family=Crimson+Pro:ital,wght@0,300;0,400;1,300;1,400&display=swap');
```
- **Cinzel** — headers, labels, nav, location titles, anything structural
- **Crimson Pro** — all body text, narration, dialogue

### CSS Variables (root — always the same)
```css
:root {
  --silver: #d6e4f0;
  --silver-dim: #7a9ab0;
  --silver-glow: #b8d4e8;
  --bg: #04060c;
  --card: #080c14;
  --text: #ccdae8;
  --text-dim: #6a8a9a;
  --border: rgba(180,220,240,0.15);
  --glow-color: rgba(180,220,255,0.18);
}
```

### Atmosphere Effects
Every game uses these — they establish the Kalmirth night-world feel:

| Effect | Class / Element | Description |
|--------|----------------|-------------|
| Bioluminescent pulse | `#glow-layer` | Slow radial glow breathing in/out every 6s |
| Floating motes | `.mote` (JS-generated) | Small light particles drifting upward, fade in/out |
| Glowing trees | `.glow-tree`, `.glow-tree-trunk`, `.glow-tree-canopy` | CSS tree silhouettes with box-shadow glow |
| Crystal spires | `.spire` | Triangle CSS shapes with drop-shadow |
| NPC silhouettes | `.npc`, `.npc-head`, `.npc-body` | Dark figure shapes, opacity ~0.6 |
| Ward rings | `.ward-ring` | Circular borders pulsing outward |
| Scene overlay | `#scene-bg::after` | Dark vignette at top and bottom edges |

### Layout Structure
```
<body>
  <nav>              ← title + back/forward links
  <div #scene-container>
    <div #scene-bg>  ← background image
    <div #glow-layer> ← ambient pulse
    <div #location-header> ← scene location label (JS-injected)
    <div #narrative-area>
      <div #scene-desc> ← main narration paragraph (JS-injected)
    </div>
    <div #nav-row>   ← Back / Continue buttons
    <div #progress-dots> ← dot indicators per scene
    <div #spot-panel> ← slides up when hotspot clicked
    <div #final-moment> ← ending screen (hidden until last scene)
    <div #transition-overlay> ← black flash between scenes
  </div>
```

---

## JavaScript: Scene Data

All scenes are in a `const SCENES = [...]` array. Each scene object:

```js
{
  id: 'scene-id',           // unique string
  location: 'Place · Time', // shown as location label (Cinzel caps)
  desc: 'Narration...',     // first-person paragraph shown in narrative area
  spots: [                  // interactive hotspots (0 or more)
    {
      id: 'spot-id',        // unique, used for localStorage tracking
      x: '60%',             // CSS left position on scene
      y: '45%',             // CSS top position on scene
      label: 'Short Label', // text on the hotspot button
      title: 'Panel Title', // heading in the slide-up panel
      text: 'Panel text...' // paragraph(s) shown in the panel
    }
  ],
  npcs: [                   // NPC silhouettes (0 or more)
    { right: '30%', bottom: '0', height: '44px', color: '#080e1c' }
  ],
  img: 'filename.jpeg'      // background image (from /game-bg/)
}
```

### Final Moment
After the last scene, `#final-moment` shows. Contains:
- A closing paragraph in italic Crimson Pro
- A fine divider
- A final character note in Cinzel caps (name, location, status)
- "Walk again" button + "Back to Games" link

---

## Voice and Tone

**Always first-person.** The player is inhabiting the character, not watching them.

**Avery's voice (established canon from vleyrra game):**
- Self-aware and slightly wry: *"This is, apparently, who I am."*
- Does not perform strength — admits fear, exhaustion, not-knowing
- Notices small details with disproportionate focus (counting root pulses, thinking about whether the bread is good)
- Doesn't say goodbye to Callen properly. Keeps thinking about that.

**Tone rules for all characters:**
- No purple prose — the world is atmospheric but the narration is grounded
- Interior monologue, not stage direction — feelings, not actions described from outside
- Dialogue in spots uses line breaks for breath, not paragraph walls
- The hotspot text should feel like a secret — something the character only admits when you look closely

**Scene length:** 1–2 paragraphs of narration per scene. Not more. Let the image and atmosphere do work.

---

## Save System

```js
const SAVE_KEY = 'kalmirth_games';

// Shape:
{
  vleyrra: { spotsRead: ['spot-id-1', 'spot-id-2'] },
  blue_river: { spotsRead: [] },
  // ... one key per game
}
```

`markSpotRead(id)` — called when a spot panel is closed. Spots already read can be visually marked (dim/checked) on revisit.

---

## Building a New Game — Checklist

1. **Choose the moment** — one emotional beat, not a plot summary. Ask: *what does this feel like from inside?*
2. **Choose the character's voice** — Avery, Callen, Fran, Nate, Isaac. Each sounds different.
3. **Write 4–6 scenes** — each with a location, narration paragraph, 1–3 hotspots
4. **Source or generate backgrounds** — drop into `/game-bg/`
5. **Copy the vleyrra `index.html`** as a starting template
6. **Replace `SCENES` array and `#final-moment` text**
7. **Update `SAVE_KEY` object** to include the new game key
8. **Add the game to `Games/index.html`** hub list

---

## Candidate Scenes (Approved for Development)

| Game | Character | Moment | Priority |
|------|-----------|--------|----------|
| Blue River Festival | Avery | Jane singing "Ode to River Goddess," Silas watching, Avery realizing she has to let go | High |
| Mythanar's Cave | Callen | The puzzle. The mannequin. The choice. Faeron watching from outside. | High |
| The Night Everything Changed | Avery | Blue River attack — the parents frozen, Bram dying, running into the dark | High |
| Two Years (Aysel) | Callen | Parallel to Avery's Vleyrra arc — training under Zachariel, the cost of it | Medium |
| Fran Arrives | Fran | First moments in Kalmirth — kept her memories, doesn't know where she is | Medium |
| Ch. 63 — The Labyrinth | Avery | The Underworld. Time distortion. Meeting Fran for the first time. | Medium |
| Isaac at Syronnia | Isaac | Age 12, first kingdom, carrying a book he doesn't understand | Low (Book 3) |

---

## Connection to Main Webapp

Games are linked from `Games/index.html` (the games hub) and the hub is linked from the main `Lore Navigator/index.html`. They share visual language but are independent files — a game can be opened without the main webapp running.

Nav links in every game:
- `← Games` → `../index.html` (games hub)
- `Lore Navigator →` → `../../index.html` (main webapp)

# Mythanar's Cave — WASD Walking Sim Plan

> A real-time first-person exploration game using raycasting (Wolfenstein 3D style).  
> Walk through the cave with WASD + mouse look. Experience the puzzle. Make the choice.

---

## Concept

You are Callen. You are inside the cave. The cave is dark, wet, and ancient. There are five plates. There is a cage. There is a mannequin. The water is rising.

**Controls:**
- `W` / `↑` — Move forward
- `S` / `↓` — Move backward
- `A` / `←` — Strafe left
- `D` / `→` — Strafe right
- `Mouse` — Look around (pointer lock)
- `E` — Interact / Examine
- `ESC` — Pause menu

---

## Technical Approach

### Raycasting Engine (No Libraries)

Custom JS raycasting renderer on HTML5 Canvas:

```js
// Core loop
function gameLoop(timestamp) {
  handleInput();
  updatePlayer();
  castRays();
  renderWalls();
  renderFloorCeiling();
  renderSprites();  // plates, cage, mannequin, NPC
  renderHUD();
  requestAnimationFrame(gameLoop);
}
```

### Map Structure

```
const MAP = [
  [1,1,1,1,1,1,1,1],
  [1,0,0,0,0,0,0,1],
  [1,0,0,0,0,0,0,1],
  [1,0,0,0,0,0,0,1],
  [1,1,1,0,0,1,1,1],
  // ... etc
];
// 1 = wall, 0 = floor, 2+ = interactive objects
```

### Room Layout (6 Rooms)

| Room | Purpose | Objects |
|------|---------|---------|
| 0 | Entrance | Stone carvings (spotlight) |
| 1 | Corridor | Water puddles (ambient sound) |
| 2 | Flooded Chamber | 5 plates (glow), cage above |
| 3 | Puzzle Room | Plates close up, pedestal |
| 4 | The Dome | Amulet pedestal, dry floor |
| 5 | Exit | Stairs up, light visible |

---

## Visual Design

### Color Palette
```css
--cave-dark: #020406;      /* deepest shadows */
--cave-stone: #1a2030;    /* wall color */
--cave-water: #0a2040;    /* water glow */
--glow-blue: #4a8ab0;      /* elven glow */
--glow-plate: #6ab0d0;    /* plate light */
--glow-dome: #8ad0f0;     /* final chamber */
--text: #b8d0e0;
```

### Atmosphere
- Distance fog (walls fade to black with depth)
- Blue point lights from plates (affects nearby wall shading)
- Ambient drips (sound, no visual)
- Vignette overlay

---

## Game Phases

### Phase 1 — Exploration
- Walk through rooms 0-2
- Examine stone carvings (E prompt appears)
- Water rises as you explore (visual change, no fail state)
- Cage descends slowly overhead (sprite)

### Phase 2 — The Puzzle
- Room 3: Stand at each of 4 plate positions
- Each plate has a symbol (displayed in HUD)
- Correct sequence: [Earth, Water, Fire, Air] — displayed on plates
- Standing on plate = "activated" (glow brightens)
- Wrong sequence = nothing happens (can retry)

### Phase 3 — The Choice
- After 4 plates activated, cage is very close
- Mannequin is visible and detailed
- Fifth interaction: `E` to sacrifice mannequin OR `E` to refuse
- Both paths work — different endings

### Phase 4 — Resolution
- Sacrifice: cage shatters, mannequin falls, no amulet
- Refuse: cage falls gently, mannequin dissolves, amulet appears
- Walk to pedestal, collect amulet
- Walk to exit stairs

---

## Sprite Objects

### Interactive Objects
```js
const SPRITES = [
  { id: 'carving', x: 3.5, y: 1.5, type: 'examine', text: 'Elven script...' },
  { id: 'plate1', x: 5.0, y: 3.5, type: 'puzzle', symbol: 'earth' },
  { id: 'plate2', x: 4.0, y: 4.5, type: 'puzzle', symbol: 'water' },
  { id: 'plate3', x: 3.0, y: 4.5, type: 'puzzle', symbol: 'fire' },
  { id: 'plate4', x: 2.0, y: 3.5, type: 'puzzle', symbol: 'air' },
  { id: 'cage', x: 3.5, y: 3.5, type: 'static', height: 1.5 },
  { id: 'mannequin', x: 3.5, y: 3.5, type: 'choice' },
  { id: 'amulet', x: 3.5, y: 4.5, type: 'collect' },
  { id: 'faeron', x: 0.8, y: 1.0, type: 'npc' }, // at entrance
  { id: 'stairs', x: 3.5, y: 5.5, type: 'exit' },
];
```

### Faeron NPC
- Only visible when looking back toward entrance
- Appears as tall dark silhouette
- If player walks close, text appears: *"You turn back. Nothing. Only dark."*
- Canon: he's there, watching, but you can't reach him

---

## HUD Elements

```
┌─────────────────────────────────────────────────────────┐
│                                                         │
│  MYTHANAR'S CAVE                    [E] Examine         │
│                                                         │
│                                                         │
│                    ████████████                         │
│                  ██            ██                       │
│                 █   PLAYER VIEW   █                     │
│                  ██            ██                       │
│                    ████████████                         │
│                                                         │
│                                                         │
│  ─────────────────────────────────────────────────────  │
│  Water is rising...                                     │
│  PLATES: ○ ○ ○ ○  (4/4 activated)                      │
│  AMULET: Not yet                                       │
└─────────────────────────────────────────────────────────┘
```

---

## Interactions

### Examining Objects
```
[E] Examine prompt appears when facing an object
Press E → text box slides up from bottom with description
Press E or click → dismiss
```

### Plate Puzzle
```
Each plate glows when you stand near it
Stand on plate → auto-activates after 1 second
HUD shows: PLATES: ● ● ○ ○
```

### The Choice
```
Cage has descended. Mannequin is close.

[E] SACRIFICE THE MANNEQUIN
Press SHIFT+E to REFUSE

Two buttons on screen, or keyboard prompt
```

---

## Audio (Optional Enhancement)

If audio implemented:
- Ambient drip (loop)
- Water rising (loop, increases volume)
- Plate activation (chime)
- Mannequin dissolve (whoosh)
- Faeron presence (distant whisper)

*If no audio: visual cues carry all atmosphere*

---

## Files

```
Games/
  mythanar/
    index.html      ← main game file
    raycast.js      ← raycasting engine (extracted)
    levels/
      cave.json     ← map data, sprite positions
    assets/
      textures/     ← optional wall textures (CSS gradients fallback)
    PLAN.md
```

---

## Implementation Order

1. **Raycasting canvas setup** — black screen, player position, basic rendering
2. **Movement** — WASD, collision detection
3. **Wall rendering** — raycasting with fog
4. **Map layout** — rooms 0-5 with correct geometry
5. **Sprites** — plates, cage, mannequin as sprites
6. **Interactions** — E to examine, proximity detection
7. **Puzzle logic** — plate sequence tracking
8. **Choice system** — sacrifice/refuse branches
9. **Resolution** — endings, amulet pickup, exit
10. **Polish** — fog, glows, HUD, Faeron cameo
11. **Playtest** — adjust movement speed, puzzle timing

---

## Performance Target

- 60 FPS on modern browsers
- Canvas resolution: 640x480 (scaled up with CSS for crispness)
- No external dependencies (pure JS + Canvas)

---

## Difficulty Considerations

- Movement speed: slow enough to feel atmospheric, fast enough not to frustrate
- Puzzle: obvious enough without walkthrough, requires attention
- Choice: clear UI, no accidental picks
- Length: ~10-15 minutes for full playthrough

# Deepborn Siege — Tower Defense Plan

> Based on Book 2: *Echoes of the First Breach*  
> Defend the Alliance base from Deepborn incursions.

---

## Lore Context

The Blue River's glow is dying. The seals are failing. The Mindless Horde breaks through at the southern border. You are a commander of the Night Crystal Alliance. Place your towers. Hold the line.

**Factions:**
- **Crystal** (Avery) — slows enemies, creates barriers
- **Light** (Nate/Sanvi) — pure damage
- **Nature** (Thunder) — poison, slows
- **Fire** (Fire mages) — burst damage, splash
- **Druid** (Silene) — roots, stuns

**Enemies:**
- Mindless drones (basic)
- Deepborn scouts (fast)
- Volmirth (armored, slow)
- Zade's shadow scouts (invisible until close)

---

## Game Structure

### Grid
- 16x12 tile grid
- Path winds from left edge to right edge (the breach)
- Player places towers on non-path tiles
- Start with 100 gold, earn gold per kill

### Towers

| Tower | Cost | Damage | Speed | Special |
|-------|------|--------|-------|---------|
| Crystal | 50 | 5 | slow | Slows enemies 30% |
| Light | 75 | 15 | fast | Long range |
| Nature | 60 | 8 | medium | Poison (DoT) |
| Fire | 100 | 25 | medium | Splash damage |
| Druid | 80 | 3 | slow | Roots enemies (stun) |

### Enemies

| Enemy | HP | Speed | Gold | Special |
|-------|-----|-------|------|---------|
| Drone | 30 | normal | 5 | Basic |
| Scout | 20 | fast | 8 | Quick |
| Warrior | 80 | slow | 15 | Tank |
| Volmirth | 150 | slow | 30 | Armored (50% resist) |
| Shadow | 40 | fast | 20 | Invisible until 2 tiles away |

### Waves
- 20 waves
- Each wave: more enemies, tougher types
- Every 5th wave: boss (Volmirth or swarm)
- Wave 10: "The Breach" — massive swarm
- Wave 20: "The Last Stand" — all enemy types

---

## UI Layout

```
┌─────────────────────────────────────────────────────────┐
│  DEEPBORN SIEGE           Wave: 3/20    Gold: 150      │
├─────────────────────────────────────────────────────────┤
│                                                         │
│  [Tower Selection Bar - 5 tower buttons]                 │
│                                                         │
│  ┌───────────────────────────────────────────────────┐  │
│  │                                                    │  │
│  │                  GAME GRID                         │  │
│  │              (16x12 tiles)                        │  │
│  │                                                    │  │
│  │    Path winds from left to right                   │  │
│  │                                                    │  │
│  └───────────────────────────────────────────────────┘  │
│                                                         │
│  [Start Wave]    Lives: 10    Score: 0                 │
└─────────────────────────────────────────────────────────┘
```

---

## Gameplay Loop

1. **Build Phase** — Place/upgrade towers before wave
2. **Wave Phase** — Enemies spawn and march toward base
3. **Towers Attack** — Auto-target nearest enemy in range
4. **Earn Gold** — Per enemy killed
5. **Repeat** — Until wave 20 or lives reach 0

---

## Tower Placement

- Click tower in selection bar → enters placement mode
- Hover over grid → shows valid/invalid tiles
- Click valid tile → places tower, deducts gold
- Right-click or ESC → cancels placement
- Click existing tower → upgrade menu

### Upgrades (3 levels each)

| Level | Cost | Effect |
|-------|------|--------|
| 1 | Base | Base stats |
| 2 | 50% base | +25% damage, +10% range |
| 3 | 100% base | +50% damage, +25% range, special boost |

---

## Visual Design

### Colors
```css
--bg-dark: #020408;
--tile-floor: #0a0f15;
--tile-path: #0f1520;
--tile-border: #1a2535;
--gold: #c8a030;
--crystal: #4a9ad0;
--light: #f0e080;
--nature: #50a050;
--fire: #d05030;
--druid: #8060a0;
--enemy: #602020;
--volmirth: #8040a0;
--shadow: #303040;
--hp-bar: #20a040;
--hp-bg: #1a1a1a;
```

### Effects
- Towers glow with faction color
- Projectiles: colored lines/particles
- Enemy death: fade out
- Crystal tower: ice particle effect
- Fire tower: ember particles
- Roots: green tendrils

---

## Game States

- **Menu** — Title, Start button, How to Play
- **Build** — Place towers, prep for wave
- **Wave** — Enemies moving, towers firing
- **Wave Complete** — Brief pause, gold bonus
- **Game Over** — Lives = 0, show score, restart
- **Victory** — Wave 20 complete, celebration, score

---

## Features

### Core
- [ ] Grid rendering with path
- [ ] Tower placement (5 types)
- [ ] Tower targeting (nearest enemy)
- [ ] Enemy pathfinding (follow path tiles)
- [ ] Wave spawning system
- [ ] Gold economy
- [ ] Lives system
- [ ] Win/lose conditions

### Polish
- [ ] Projectile animations
- [ ] Enemy health bars
- [ ] Tower range indicator on hover
- [ ] Wave announcement
- [ ] Score tracking
- [ ] Sound effects (optional)

---

## Files

```
Games/
  deepborn-siege/
    index.html    ← all-in-one game
    PLAN.md
```

---

## Technical Notes

- Pure HTML/CSS/JS, no libraries
- Canvas for game grid and entities
- requestAnimationFrame game loop
- Grid-based collision (not pixel-perfect)
- Simple A* or direct path following
- ~500-800 lines total

---

## Future Additions

- Hero units (Avery, Nate, Thea) as special towers
- Volmirth ally mechanic (wave 15+)
- Seal repair mini-game between waves
- Dark elf boss (Zade) in wave 18

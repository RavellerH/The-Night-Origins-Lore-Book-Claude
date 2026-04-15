# Thea's Stand — Hack & Slash Plan

> Based on Book 2: *Echoes of the First Breach*  
> Play as Thea Holt, Commander of Nordell Border Forces. No magic. Just steel and will.

---

## Lore Context

Thea Holt has fought the Deepborn for 20 years without magic. She's seen things the mages haven't. She knows these creatures aren't just mindless — something is watching. Something is waiting.

You are Thea. The breach is open. They're coming through. You will hold this line alone until backup arrives.

---

## Game Concept

**Top-down hack & slash survival**

- Move with WASD
- Click to swing sword in that direction
- Survive waves of Deepborn enemies
- No magic — pure combat skill
- Earn points for kills
- Upgrade between waves

---

## Player: Thea Holt

- Represented as armed soldier silhouette
- Sword swing arc (click to attack)
- Dash ability (Spacebar) — short cooldown
- Health bar
- Stamina bar (for dashing)

---

## Enemies

| Enemy | HP | Damage | Behavior |
|-------|-----|-------|----------|
| Drone | 20 | 5 | Basic, walks toward player |
| Scout | 15 | 8 | Fast, circles then strikes |
| Warrior | 50 | 15 | Slow, heavy hits |
| Volmirth | 100 | 25 | Armored, charges |
| Shadow | 25 | 10 | Teleports behind player |

---

## Controls

- `WASD` — Move
- `Mouse` — Face direction
- `Left Click` — Sword swing (arc attack)
- `Space` — Dash (costs stamina)
- `E` — Use health potion (if equipped)

---

## Wave System

- 15 waves of increasing difficulty
- Each wave: more enemies, tougher types
- Every 5th wave: Volmirth boss
- Between waves: brief shop/upgrade phase

---

## Upgrades (Shop)

| Upgrade | Cost | Effect |
|---------|------|--------|
| Health Potion | 50 | Restore 50 HP |
| Max Health +20 | 100 | Permanent +20 HP |
| Stamina +1 | 75 | +1 dash charge |
| Sword Range | 125 | +20% attack range |
| Sword Damage | 150 | +25% damage |
| Armor | 200 | -10% damage taken |

---

## Scoring

- Kill drone: 10 pts
- Kill scout: 15 pts
- Kill warrior: 25 pts
- Kill volmirth: 50 pts
- Kill shadow: 30 pts
- Wave clear bonus: 100 × wave number

---

## Visual Design

### Colors
```css
--bg: #020408;
--ground: #0a0f15;
--thea: #c0a060;      /* Thea's armor color */
--sword: #e0e0e0;      /* Sword gleam */
--sword-arc: rgba(200, 180, 120, 0.4);
--enemy-drone: #602020;
--enemy-scout: #804030;
--enemy-warrior: #a04040;
--enemy-volmirth: #8040a0;
--enemy-shadow: #303040;
--hp-bar: #c04040;
--stamina-bar: #4080c0;
--ui-bg: rgba(10, 15, 25, 0.9);
```

### Effects
- Sword swing: white arc slash
- Enemy death: fade + particle burst
- Dash: blur trail
- Volmirth charge: purple glow
- Shadow teleport: fade in/out

---

## UI Layout

```
┌─────────────────────────────────────────────────┐
│ THEA'S STAND          Wave: 3/15    Score: 450  │
├─────────────────────────────────────────────────┤
│                                                 │
│                   [GAME ARENA]                   │
│              Thea vs Deepborn horde             │
│                                                 │
├─────────────────────────────────────────────────┤
│ [HP ████████░░] [STAMINA ██████░░░░]   [POT: 2] │
│ [SPACE] Dash    [E] Potion     [SHOP]          │
└─────────────────────────────────────────────────┘
```

---

## Game States

1. **Title** — Story intro, start button
2. **Game** — Active combat
3. **Wave Clear** — Brief pause, shop opens
4. **Shop** — Buy upgrades
5. **Game Over** — Death screen, score, restart

---

## Files

```
Games/
  theas-stand/
    index.html    ← all-in-one game
    PLAN.md
```

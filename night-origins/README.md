---
tags:
  - lore-navigator
  - night-origins
  - interactive-game
  - writing
created: 2026-04-04
---

# Night Origins — Interactive Game

> **Game file:** `Lore Navigator/Games/night-origins/index.html`
> **Style model:** `Lore Navigator/Games/vleyrra/index.html`
> **Canon source:** `_script7_extracted.txt` + Beat Sheet Part 1

A chapter-by-chapter interactive story game covering *The Night Origins* from the beginning — prologue through the Black Leaf assault on Blue River Village. Connects directly to the [[Vleyrra — The First Night]] game at the end.

---

## How to Play

Open `Lore Navigator/Games/night-origins/index.html` in a browser.
- Select a chapter from the chapter select screen
- Navigate scenes with **Continue →** / **← Back** or arrow keys
- Click glowing spot markers for expanded prose (character interiority, world details)
- Chapter VI ends with a link to the Vleyrra game

---

## Chapter Overview

| Chapter | Title | POV | Purpose |
|---------|-------|-----|---------|
| **Prologue** | Night Etdler | World voice | Set the world before Avery appears |
| **I** | The Blue River | Avery | Crystal appears; sibling dynamic established |
| **II** | The Group at the Oak | Avery | Social wound; Kathrine; loneliness |
| **III** | The Brothers | Nathaniel | Dramatic irony — player sees what Avery doesn't |
| **IV** | Home | Avery | Crystal revealed to adults; stakes shift |
| **V** | Mythanar's Cave | Avery | Last warm night; the puzzle; the bond |
| **VI** | The Night | Assault → Avery | Everything breaks |

---

## POV Structure

- **World voice** (Prologue): Omniscient, cool, no character interiority. Sets up world facts as given.
- **Avery (first person)**: Dry, precise, hides anxiety with accuracy. Removes filter words — direct experience, no "she felt."
- **Nathaniel (third person close)**: Quieter register. Fewer jokes. Longer silences. Narrator as contained as he is.
- **Assault sequence** (Chapter VI): Opens in stripped third POV (no interiority, cold record) then snaps back to first-person Avery.

---

## Connecting to Vleyrra

The final scene of Chapter VI leads directly into [[Vleyrra — Avery's First Night]]. The crystal she holds while running south is the same crystal from the river — still warm, still amber — while everything else she made that night was cold. The warmth is the thread that connects both games.

---

## Files in This Folder

- [[chapters]] — full scene descriptions and spot text for all 7 chapters
- [[image-prompts]] — AI image generation prompts for each scene (consistent with vleyrra style)

---

## Technical Notes

- Single HTML file, no external dependencies beyond Google Fonts
- Same `localStorage` save key as vleyrra (`kalmirth_games`, sub-key `night_origins`)
- Background images: `../../game-bg/no-[scene-id].jpeg` — CSS gradient fallbacks until images added
- Mote particle color and glow color shift per chapter (warm → cold → assault red)
- Chapter select → chapter title card → scenes is the navigation flow

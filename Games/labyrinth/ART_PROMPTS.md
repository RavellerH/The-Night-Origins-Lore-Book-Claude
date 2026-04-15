# Labyrinth — Art Generation Prompts

This document provides consistent, detailed prompts for generating game assets using the Kalmirth "Style Anchor."

## 🎨 The Universal Style Anchor
Append this to **every** prompt to maintain consistency:
> *dark fantasy concept art, painterly digital illustration, impressionistic brushwork, volumetric atmospheric light, deep shadow and muted rich palette, dramatic perspective — style of: Jakub Różalski, Jessica Rossier, Marc Simonetti*

---

## 1. Environment & Backgrounds (16:9 Aspect Ratio)

### Title Screen (`bg_title.png`)
> **Prompt:** A vast, vertically oriented underworld cavern. In the center, an ancient dark purple tree with stone-like branches reaches toward a cave ceiling. Far below, a river of orange-red lava provides a hellish glow. In the midground, a shimmering violet spatial distortion portal. The scale is immense, emphasizing the oppressive weight of the stone. Palette: Deep violet, obsidian black, lava orange. *[+ style anchor]*

### Floor 1-2: Demon Corridors (`bg_floor_1.png`)
> **Prompt:** Inside a narrow labyrinth corridor made of jagged obsidian stone. Glowing purple crystalline veins pulse in the walls like a heartbeat. Volumetric purple haze fills the air, with floating ash and embers. The floor is cracked stone. Shadows are long and ink-black. Palette: Midnight purple, silver-blue highlights, obsidian grey. *[+ style anchor]*

### Floor 3-4: Deepborn Taint (`bg_floor_3.png`)
> **Prompt:** A cavernous labyrinth hallway overtaken by sickly green organic growth. Pulsating dark vines crawl across the ceiling. Distorted geometry where the stone seems to be melting or twisting. Oppressive, heavy atmosphere. Palette: Sickly moss green, deep shadow, pale white bioluminescent spores. *[+ style anchor]*

---

## 2. Character Portraits (1:1 or 2:3, Waist-up)

### Avery — Crystal Origin (`port_avery.png`)
> **Prompt:** A waist-up portrait of a 18-year-old woman with flowing midnight-blue hair and piercing sky-blue eyes. She wears silver-trimmed light mage robes that look travel-worn. She holds a floating, glowing blue crystal shard. Her expression is calm but carries the weight of a heavy discovery. Soft blue light reflects on her face. Palette: Silver, midnight blue, cold crystal blue. *[+ style anchor]*

### Nate — Light Mage (`port_nate.png`)
> **Prompt:** A waist-up portrait of a young man with messy brown hair and warm amber eyes. He wears gold and white mage vestments with leather straps. His hands are raised slightly, radiating a soft golden light that cuts through deep shadows. His expression is protective and determined. Palette: Warm gold, cream white, deep amber. *[+ style anchor]*

---

## 3. Bestiary: Battle Sprites (1:1, Full Body, Neutral Background)

### Corrupted Imp (`btl_imp.png`)
> **Prompt:** A small, hunched demonic creature with leathery purple skin. Jagged, sharp crystal shards grow from its spine and shoulders. It has glowing red slit-eyes and long, blackened claws. It is caught in a predatory crouch. Minimal background. Palette: Bruised purple, crimson red eyes, crystal blue shards. *[+ style anchor]*

### Demon Scout (`btl_scout.png`)
> **Prompt:** A tall, slender humanoid demon wearing scorched elven-style plate armor. It holds a spear made of dark purple crystal. A single glowing red eye visible through a helmet slit. It is surrounded by a faint aura of shadow smoke. Palette: Charred metal, deep violet, glowing red. *[+ style anchor]*

---

## 4. UI & Icons (Technical Prompts)

### Labyrinth Rune Fragment (`icon_rune.png`)
> **Prompt:** A closeup concept study of a single cracked stone tablet. Carved into the stone is a glowing elven rune that emits a soft blue light. The stone texture is weathered and ancient. Background is blurred dark stone. *[+ style anchor]*

### Labyrinth UI Frame (`ui_frame.png`)
> **Prompt:** A rectangular frame design for a game menu. Made of dark tarnished silver and filigree patterns of purple vines. The corners are capped with small blue crystals. Dark fantasy aesthetic, elegant but aged. *[+ style anchor]*

---

## 🏗️ Technical Implementation Note
When generating with Google Nano/Banana:
1.  **Resolution:** 1024x1024 for portraits/bestiary, 1344x768 for backgrounds.
2.  **Negative Prompt:** *cartoon, anime, 3d render, plastic, neon, bright colors, happy, modern, technology, low resolution, blurry.*
3.  **Post-Processing:** I will use CSS filters (e.g., `sepia(0.2) contrast(1.1)`) in `index.html` to further unify the look of these images once they are loaded.

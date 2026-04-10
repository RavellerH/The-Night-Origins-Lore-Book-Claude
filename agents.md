# Lore Navigator — Agent Update Protocol

This file is the instruction manual for any Claude agent tasked with keeping `index.html` in sync with the lore files in `../Lore/`.

---

## What This Webapp Is

`index.html` is a self-contained lore navigator for the Kalmirth series.
All lore content is embedded as JavaScript objects inside the `<script>` block — specifically inside the `const LORE = { ... }` object and the `const SEARCH_INDEX = [...]` array.

There is no server or build step. The file is opened directly in a browser.

---

## When to Run an Update

Run an update agent any time one or more of the following changes:

| Lore File Changed | Affects |
|---|---|
| `Lore/CHARACTERS.md` | `LORE.characters` section, `SEARCH_INDEX` character entries |
| `Lore/FACTIONS.md` | `LORE.factions` section, `SEARCH_INDEX` faction entries |
| `Lore/MAGIC_SYSTEM.md` | `LORE.magic`, `LORE.origins` sections, search entries |
| `Lore/ORIGINS_SYSTEM.md` | `LORE.origins` section, search entries |
| `Lore/BLACK_LEAF.md` | `LORE.blackleaf` section, search entries |
| `Lore/KINGDOMS_AND_LOCATIONS.md` | `LORE.locations` section |
| `Lore/WORLD_GEOGRAPHY.md` | `LORE.geography` section |
| `Lore/PLOT.md` | `LORE.plot` section |
| `Lore/TIMELINE.md` | `LORE.timeline` section |
| `Lore/DEEPBORN.md` | `LORE.deepborn` section |
| `Lore/GREAT_WAR.md` | `LORE.greatwar` section |
| `Lore/RELIGION_AND_BELIEF.md` | `LORE.religion` section |
| `Lore/Open_Questions.md` | Note in relevant sections as open questions |
| Any character file in `Night Origins/Character Design/` | `LORE.characters` entries |

---

## How to Update — Step by Step

### Step 1 — Read the changed lore file
Use the Read tool to get the full current content of the changed file.

### Step 2 — Locate the matching section in `index.html`
Open `index.html` and find the relevant `render()` function inside `const LORE = { ... }`.
Example: changes to `CHARACTERS.md` → find `characters: { ... render() { return \`...\` } }`.

### Step 3 — Update the render() function
Edit the `render()` function to reflect the new or changed lore. Rules:
- Keep existing HTML structure (parchment cards, char-cards, faction-cards, tables)
- Add new characters as `<div class="char-card">` blocks
- Add new factions as `<div class="faction-card">` blocks
- Update tables with new rows
- For new top-level sections, add both a `nav-item` in the sidebar AND a new key in `LORE`

### Step 4 — Update SEARCH_INDEX
At the bottom of the `<script>` block, find `const SEARCH_INDEX = [...]`.
Each entry has this shape:
```js
{ page: 'characters', section: 'Character Name', text: 'space separated keywords for search' }
```
Add or update entries for any new characters, locations, factions, or concepts.
Keep `text` as a space-separated keyword string (not sentences — just keywords the user might search for).

### Step 5 — Verify
Check that:
- All `[[WIKI_LINK]]` references in the lore are converted to `<a class="xref" onclick="navigate('pageId')">` links
- No Markdown syntax (`##`, `**`, `-`) is left raw in the HTML strings
- The file still opens correctly in a browser (no JS syntax errors)

---

## HTML Structure Reference

```
const LORE = {
  pageId: {
    title: "Display Title",
    breadcrumb: "Category Label",
    render() {
      return `<div class="parchment"><div class="parchment-inner">
        ...HTML content...
      </div></div>`;
    }
  },
  ...
}
```

### CSS Classes Available

| Class | Use |
|---|---|
| `parchment` / `parchment-inner` | Outer/inner parchment card wrapper |
| `section-title` | H1-level heading with icon |
| `subtitle` | H2-level heading with gold left border |
| `subtitle-sm` | H3-level heading |
| `char-card` | Character or entity info block |
| `char-name` / `char-role` | Name and role inside char-card |
| `faction-card` / `faction-header` / `faction-body` | Faction block |
| `faction-header.light/nature/shape/dark/fire/bl` | Faction color variants |
| `lore-table` | Styled table |
| `note` | Italicized callout block |
| `ornament` | Decorative divider line |
| `tag` | Small label badge (add `.gold`, `.red`, `.dark`, `.green`, `.purple`) |
| `timeline-entry` / `timeline-when` / `timeline-dot` / `timeline-text` | Timeline row layout |
| `xref` | Cross-reference link (onclick navigate) |
| `stat-row` | Flex row of tags under a char name |

---

## Adding a New Page

1. Add a new `nav-item` in the sidebar HTML:
   ```html
   <div class="nav-item" data-page="newpage">
     <span class="icon">🔮</span> New Page
   </div>
   ```

2. Add a new entry in `const LORE = { ... }`:
   ```js
   newpage: {
     title: "New Page Title",
     breadcrumb: "Category",
     render() { return `<div class="parchment"><div class="parchment-inner">...</div></div>`; }
   },
   ```

3. Add search entries to `SEARCH_INDEX`.

4. If it appears on the home grid, add a tile to `LORE.home.render()`.

---

## Do Not Change

- The CSS `:root` variables and all CSS rules (style changes are a design task, not a lore sync task)
- The `navigate()`, `doSearch()`, and event-listener logic
- The sidebar collapse behavior or topbar
- The Google Fonts import URL

---

## Source of Truth

The lore files in `../Lore/` are always the source of truth.
`index.html` is a read-only display layer — it shows what the lore says; it does not define it.
When a lore file and the webapp conflict, the lore file wins.

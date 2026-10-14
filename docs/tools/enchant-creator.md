# Enchant Creator

`tools/enchant-creator.html` is a standalone, offline editor for your `enchants/` folder — open it in Chrome or Edge, click **Open plugin folder** and pick `plugins/JPPrison`. No add-ons, no server and no installation needed.

!!! tip "Where to get it"
    The tool ships in the plugin repository under `tools/enchant-creator.html`. You can keep it anywhere on your disk and open it any time.

## Beginner wizard (default tab)

Press **New** and follow five friendly steps:

1. **Pick what the enchant does** from 35 illustrated idea cards — every mining blast, money bags, key givers, token/gem/XP boosters, combo master, classic enchantments, potion effects...
2. **Name it and choose an icon** from the searchable gallery.
3. **Tune it with sliders** while the in-game item preview and a plain-language description update live.
4. Optionally **enable a balanced auto-generated prestige ladder**.
5. **Save.** New enchants get a file name derived from their display name automatically.

No YAML, no field names, no server knowledge required. The tool is bilingual — toggle **EN/ES** in the top right.

## 3D mine editor

A voxel mine renders next to the wizard and plays the exact break pattern of the selected effect (the same radius, depth and speed the plugin uses), so you see what players will see before saving.

- Drag to orbit, wheel to zoom.
- Click any block to choose where the enchant triggers.
- **Fullscreen** opens the big editor with a timeline scrubber: step through the animation frame by frame, switch playback speed and the mine's block material.

Only behaviors the plugin actually supports are previewed — what you see is what the imported enchant does.

## Free builder (Combine & customize)

Turn any idea into something original:

- Set or clear the mining effect (all 17 blasts)
- Add a passive reward multiplier (any economy or pickaxe XP)
- Map a classic enchantment
- Attach a fixed-id special (keys, money bag, sell booster, lucky, combo, haste/speed/jump/night vision)
- Edit console commands, activation FX and the head skin

Cards tagged **UNIQUE** are behaviors the plugin looks up by file id: the tool opens or creates that single enchant instead of cloning a dead copy.

## Advanced tab

- The full form with every field (prestige and momentum tables included)
- The raw **YAML tab**, with comment-safe saving — your comments survive edits
- The bundled templates available for cloning
- A validation panel: ids, enums, economy references, chances and prestige contiguity

Use it to fine-tune anything created with the wizard.

## Compatibility & safety

- **Chromium (Chrome/Edge)**: reads and writes files back directly (UTF-8).
- **Firefox / Safari / drag & drop**: read-only import + per-file download or a full ZIP export.
- The tool backs up the whole folder to a ZIP before bulk saves.
- After saving, run `/jpprison reload` in game to apply the changes.

!!! note "The schema stays in sync"
    A build-time test (`EnchantCreatorSchemaTest`) fails the build if a bundled enchant uses a field the tool does not know — the tool never silently loses options.

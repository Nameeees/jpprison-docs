# JPPrison

**Mine. Upgrade. Prestige. Repeat.**

An all-in-one Prison core plugin: enchanted pickaxes, multiple economies, autosell, pets, minions, robots, bombs, drills, loot chests, crates, boost armors, prestige, combos and more — in a single jar that runs from **Minecraft 1.8.8 to the latest release**, with no NMS and no required dependencies.

Everything is data-driven: enchants, economies, menus, pets, bombs, drills, minions, upgrades, loot chests, crates and animations are defined in YAML files that you can edit — and reload with `/jpprison reload` — without touching code or recompiling.

<div class="grid cards" markdown>

-   :material-rocket-launch: **New server owner?**

    ---

    Install the jar, flag your mine region and start paying rewards in minutes.

    [:octicons-arrow-right-24: Quick start](getting-started/quick-start.md)

-   :material-book-open-variant: **Reference**

    ---

    Every command, alias, permission and PlaceholderAPI variable.

    [:octicons-arrow-right-24: Commands](reference/commands.md) · [:octicons-arrow-right-24: Placeholders](reference/placeholders.md)

-   :material-code-braces: **Developer?**

    ---

    Balance API, events and WorldGuard/zone integration for your own plugins.

    [:octicons-arrow-right-24: API](developer/api.md)

-   :material-wand: **Designing enchants?**

    ---

    The Enchant Creator: visual, offline, bilingual, no YAML needed.

    [:octicons-arrow-right-24: Open the guide](tools/enchant-creator.md)

</div>

## Highlights

| System | What it does |
|---|---|
| **Pickaxe enchants** | 35 bundled enchants with a visual upgrade menu, per-enchant unlock levels, prestige, activation effects and messages. Add new ones with a YAML file. |
| **Custom economies** | Tokens, Gems and any currency you invent — each with its own `/command`, symbol, color, leaderboard, payments and a Vault bridge for shop plugins. |
| **Autosell** | Per-block prices and XP in every economy. The same price table pays your pickaxe, enchants, bombs, drills, the clone miner and minions. |
| **Destructive magic** | Explosive, nuke, thunder, meteor, cross, layer, solar, frost nova, geyser, soul spiral, comet, volcano, blackhole, aurora, sandstorm and apocalypse — progressive animations with full rewards. |
| **Bombs & drills** | Throwable TNT skulls and placeable hoppers that mine in bulk, with data-driven types and one payout summary per use. |
| **Autominer** | A visible clone wearing your skin that walks the mine and mines as if you did it yourself. Time-based, with 10 permanent upgrade modules. |
| **Minions** | A crew of miniature mobs (baby zombie, baby pigman, baby villager) mining on their own. Feed them time items; every mob has its own upgrades. |
| **Robots** | Passive economy workers that bank their currencies around the clock — online and offline — and level up while you collect them. |
| **Pets** | 13 floating-head pets with levels, per-economy boosts, mini armor, animations, particle trails and unlock lucky blocks. |
| **Boost armors** | A 10-set wearable unlock chain, each set stronger than the last, with per-level growth and live progress bars. |
| **Shards & runes** | Socketable boosts with 13 effect types (XP, rewards, sell, luck, speed, cost...) and progressive slot unlocks. |
| **Loot chests** | Mining reward chests with holograms, custom animations and sounds, claim summaries and five bonus minigames. |
| **Crates** | Giveable crate items that open with a 3D roulette show and weighted, command-driven prizes. |
| **Prestige** | Global pickaxe prestige and per-enchant prestige with tiers, costs, multipliers and bonus caps. |
| **Momentum** | Keep mining non-stop to build a combo multiplier, with milestone celebrations. |
| **Player profile & quests** | `/stats` profile cards for any player and a `/quests` daily/weekly board with click-to-claim rewards. |
| **Earnings dashboard** | The `/earn` dashboard (best source first, ranking, trends, history, details) plus automatic 1m/5m chat recaps. |
| **Leaderboards & boosts** | Top-10 for blocks, pickaxe XP and every economy — plus timed XP/currency boosts and the `/boosts` multiplier view. |
| **Quality of life** | Versioned auto-migrations, automatic backups, per-module feature flags, universal placeholders and a full YAML menu system. |

## Compatibility

| | |
|---|---|
| **Server** | Spigot / Paper **1.8.8 through the latest release** — one jar, no recompiling per version. |
| **Java** | 8 or newer. |
| **Storage** | SQLite (bundled, zero configuration). |
| **Dependencies** | None required. |

Optional integrations: **WorldGuard** 6.x/7.x+ (the `jpprison-enchants` region flag gates all mining rewards), **Vault** (expose one economy to shop plugins) and **PlaceholderAPI** (the `%jpprison_<variable>%` expansion).

## Support & license

JPPrison is developed by **MrDriz_** and licensed **free with attribution, no resale**: you may use, modify and redistribute it (including on public and for-profit servers) as long as you keep the credit, and you may not sell the plugin or modified versions. Bundled third-party components keep their own licenses: XSeries (MIT), sqlite-jdbc (Apache-2.0) and bStats (MIT).

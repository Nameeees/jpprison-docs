# Installation

## Requirements

| | |
|---|---|
| **Server** | Spigot or Paper, Minecraft **1.8.8 through the latest release**. One jar covers every version — no recompiling, no separate builds. |
| **Java** | Java 8 or newer. |
| **Storage** | SQLite — bundled and configured out of the box (`data.db` in the plugin folder, WAL journal mode). |
| **Required plugins** | None. |

### Optional integrations

| Plugin | What it adds |
|---|---|
| **WorldGuard 6.x / 7.x+** | The custom `jpprison-enchants` region flag that gates mining rewards per region. Without it, mining rewards simply work everywhere. |
| **Vault** | Exposes one of your economies (default `money`) to Vault-based shop plugins. |
| **PlaceholderAPI** | Registers the `%jpprison_<variable>%` expansion for scoreboards, holograms, tab and chat. |

## Install

1. Download `JPPrison-<version>.jar`.
2. **Stop the server.**
3. Drop the jar into your server's `plugins/` folder.
4. Start the server. On first run, every configuration file is generated under `plugins/JPPrison/`.
5. Edit the generated files to fit your server.
6. Apply changes in game with `/jpprison reload`.

!!! warning "Always swap the jar with the server stopped"
    Never overwrite `JPPrison.jar` while the server is running: the JVM keeps a handle to the old file, and lazily-loaded classes start failing with `ClassNotFoundException` after the swap. Stop the server, copy the jar, restart.

!!! tip "Reloading vs restarting"
    `/jpprison reload` reloads `config.yml`, `messages.yml`, economies, enchants, menus and most module files from disk. Some public switches (like bStats) are only read once, at startup — restart for those.

## What gets created

Everything lives in `plugins/JPPrison/`:

| File / folder | Purpose |
|---|---|
| `config.yml` | Core settings: pickaxe curve and cap, prestige, boosts, feature flags, Vault, backups, public switches. |
| `messages.yml` | All chat messages (chat only — menu UI strings live in the menu files). |
| `autosell.yml` | Per-block sell prices, XP and economy rewards — the payout table for every system. |
| `economies/` | One file per economy (`id`, `name`, `symbol`, `color`, `decimals`). |
| `enchants/` | One file per enchant (levels, costs, chances, activation FX, prestige). |
| `menus/` | Every menu layout and UI string. |
| `colors/` | Pickaxe lore themes. |
| `shards.yml` / `runes.yml` | Insertable boosts. |
| `pets.yml` | Pets, levels, boosts, armor colors, lucky blocks. |
| `armors.yml` | Boost armor chain. |
| `bombs.yml` / `drills.yml` | Throwable / placeable tool types. |
| `autominer.yml` + `autominer-upgrades/` | Clone miner settings and its upgrade modules. |
| `minions.yml` + `minions/` + `minion-upgrades/` | Mini-mob crew settings, types and modules. |
| `robots.yml` + `robots/` | Passive economy workers: global switches + one file per robot. |
| `loot-chests.yml`, `minigames.yml`, `animations.yml`, `sounds.yml` | The loot chest system. |
| `crates/` | One file per crate: item look, opening animation tuning and the weighted loot list. |
| `luckyblocks.yml` + `luckyblocks/` | Mining LuckyBlock settings and types. |
| `milestones.yml` | Block-count milestone ladder. |
| `quests.yml` | Daily & weekly quest catalog. |
| `zones.yml` | Per-region rules: disabled enchants/features per WorldGuard region, plus reward/sell/XP multipliers. |
| `orbs.yml` / `remnants.yml` | XP orb / enchant remnant item looks. |
| `animations.yml`, `sounds.yml` | Loot chest animation and sound libraries. |
| `backups/` | Automatic zip backups (database + all configs). |

Every file carries a `config-version` key and is **migrated automatically** on startup and on `/jpprison reload` — only the keys that change are touched, and your edits and comments are preserved.

## Next steps

- [Quick start](quick-start.md) — flag your mine and start paying rewards.
- [Configuration](configuration.md) — what lives where, feature flags and reloads.
- [Commands](../reference/commands.md) — the full command reference.

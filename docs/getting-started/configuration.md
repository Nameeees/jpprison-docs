# Configuration

Everything is edited in plain YAML inside `plugins/JPPrison/` and applied with `/jpprison reload` — no code involved.

## Versioned configuration & auto-migration

Every data file carries a `config-version` key. On startup and on every `/jpprison reload`, the migrator:

- reads each file and runs the step-wise migrations for its version,
- only touches the keys that changed (renames keep your values; new keys arrive with documented defaults),
- re-saves UTF-8 and re-injects the previous file's comments by YAML path, so **your notes survive the rewrite**.

You never need to delete a config to "get the new options": update the jar, run `/jpprison reload`, done.

!!! tip "Editing tips"
    - The files are documented inline — every section and key has a comment (ASCII-only comments, UTF-8 values).
    - Use `/jpprison economy` (in-game wizard) to create new economies without touching YAML.
    - `messages.yml` is **chat only**; menu titles, button names and lore live in `menus/`.

## Feature flags (master switches)

`features:` in `config.yml` — 28 module switches, all default `true` (a missing key means ON). Flip one and `/jpprison reload`; reads are live, nothing is cached.

| Switch | System |
|---|---|
| `mining` | Core mining pipeline (rewards on block break). |
| `enchants` | Pickaxe enchant procs and sweeps. |
| `autosell` | Automatic block selling. |
| `orbs` | XP orb items. |
| `remnants` | Enchant remnant items. |
| `bombs` | Throwable bombs. |
| `drills` | Placeable drills. |
| `autominer` | The clone miner (and its upgrades). |
| `minions` | The mini-mob crew. |
| `robots` | Passive economy workers. |
| `luckyblocks` | Mining LuckyBlocks. |
| `milestones` | Block-count milestone rewards. |
| `stats` | `/stats` player profiles. |
| `quests` | Daily & weekly quests. |
| `loot-chests` | Mining loot chests (including their minigames). |
| `crates` | Crate openings. |
| `shards` | Pickaxe shards. |
| `runes` | Pickaxe runes. |
| `pets` | Floating pets. |
| `armors` | Boost armors. |
| `earn-board` | The `/earn` earnings dashboard. |
| `earnings-recap` | Automatic 1m/5m chat recaps. |
| `prestige` | Global pickaxe prestige. |
| `momentum` | The combo multiplier. |
| `boosts` | The `/boosts` multiplier view. |
| `top` | Leaderboards. |
| `backups` | Automatic backups. |
| `guide` | The `/guide` reference hub. |

**Semantics of disabling one:**

- Disabled = no **new** effect: commands refuse, menus won't open, listener uses are swallowed (items are never wasted) and procs don't fire.
- In-flight state finishes naturally: a running clone or minion idles with no time charged, live loot chests stay protected and claimable, spawned pets keep following without boosts.
- **Nothing is ever deleted** — flip the flag back and your data is there.
- Always on by design: raw admin grants (`give`/`pay`/`/jpprison xp`), core economy/balances, usage lines, PlaceholderAPI and tab-completes.

## The main `config.yml` sections

| Section | What you tune there |
|---|---|
| `pickaxe` | Item name and lore, XP curve (`xp-curve-exponent`), level cap (`max-level`, 0 = infinite), progress bar, and drop protection (`drop-confirm`, `keep-on-death`). |
| `pickaxe.prestige` | Global prestige: enabled, max level, per-level economy/XP multipliers and caps, plus the optional tier list (`levels.<n>`: `required-level`, `cost`, `cost-economy`). |
| `pickaxe.xp-boosts` | Timed XP boost settings (`max-duration-minutes`, `sound`). |
| `mining` | `estimated-blocks-per-minute` — used by `/jpprison balance` estimates. |
| `economy` | Vault bridge (`vault-currency`, `vault-name`, `vault-color`) and display decimals. |
| `economy-boosts` | Timed per-economy boost settings (`max-duration-minutes`, `sound`). |
| `boosts` | `global-cap` — ceiling for the final payout multiplier at every mining payout site (0 = uncapped). |
| `features` | The master switches above. |
| `backups` | `interval-hours`, `keep`, `on-start`. |
| `update-checker` | Console update check (`enabled`, `url`, `interval-hours`) — off by default. |
| `bstats` | bStats metrics (`enabled`, `plugin-id`) — enabled by default with the project's own plugin id; opt out here or globally in `plugins/bStats/config.yml`. |
| `debug` | Verbose logging for troubleshooting placeholders and integrations. |

## Reloading

- `/jpprison reload` reloads `config.yml`, `messages.yml`, economies, enchants, menus and the module files from disk — migrations run first.
- Feature flags, zones, enchants, menus and economies apply immediately.
- A few switches are read once at startup only (like bStats) — restart for those.

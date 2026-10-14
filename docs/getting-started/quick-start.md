# Quick Start

From zero to a working prison loop in four steps.

## 1. Flag your mine (WorldGuard)

Rewards are gated by the custom `jpprison-enchants` flag. Create your mine region and enable it:

```text
/rg flag <region> jpprison-enchants allow
```

The rule in one line: **economy rewards, pickaxe XP, pet/armor XP, block counters and autosell only happen where the flag is DEFINED**.

- **`allow`** — rewards inside the region.
- **`deny`** — no rewards inside the region; blocks still break normally.
- **Outside any region** (flag undefined) — no economy rewards and no autosell: blocks drop into the inventory instead.
- **No WorldGuard installed?** There are no regions to enforce, so everything works everywhere.

Bombs, drills, the autominer clone, minions and loot chests all share the same flag check, so every destructive tool respects your mines.

!!! tip "Per-mine rules"
    Need a mine where some enchants never fire, autosell is off, or rewards pay more? That is what `zones.yml` is for — see [Configuration](configuration.md).

## 2. Price your blocks (`autosell.yml`)

`autosell.yml` is the single payout table of the server: per-block sell prices, XP and economy rewards. Every system pays out what you configure here — your pickaxe, enchant sweeps, bombs, drills, the clone miner and minions.

```yaml
prices:
  STONE:
    sell: 1.0        # autosell value (the vault/money economy)
    xp: 1            # pickaxe XP per block
    rewards:         # per-economy rewards
      tokens:
        chance: 100.0    # optional, default 100
        min: 50
        max: 150
      gems:
        chance: 8.0
        min: 10
        max: 30
```

Ores that drop items instead of blocks (coal, diamond, emerald, redstone, lapis, quartz and the 1.17+ raw ores) automatically price their drop items too — those sell out of the box.

## 3. Adjust `config.yml`

Pickaxe name and lore, XP curve and level cap, prestige tiers and costs, boost caps, the Vault bridge, feature flags, backups... See [Configuration](configuration.md) for the full map.

## 4. Give yourself a test kit

```text
/orb <player> 1000
/shard <player> power-shard 3
/jpprison bomb give <player> nuke 5
/jpprison autominer give <player> 10m
/jpprison minion give <player> zombie 10m 2
/jpprison crate give <player> legendary 1
```

## Where to go next

- [Features](../features.md) — the full system tour.
- [Commands](../reference/commands.md) — every command, alias and permission.
- [Placeholders](../reference/placeholders.md) — for scoreboards, holograms and chat.
- In game: `/guide` (admin) opens the developer reference hub, with one page per system.

# Commands

Syntax, permissions, aliases, ids and examples for every command in the plugin.

**Conventions**

- `<arg>` = required
- `[arg]` = optional
- `a|b` = choose one (the bar itself is not typed)

All player commands work without configuring anything (`jpprison.user` is default: `true`). Admin commands need `jpprison.admin` (default: op).

If a feature is disabled in the `features:` section of `config.yml`, its commands are rejected with the `features.disabled` message (e.g. `/autosell`, `/xpboost`, `/jpprison backup`, `/jpprison shard`, `/jpprison minion`...). `/jpprison reload` applies the flags instantly.

Tab-complete suggests subcommands, online players and the ids of every system (enchants, economies, bombs, drills, shards, runes, crates, pets, armors, minions, robots, luckyblocks).

## 0. Permission summary

Full node-by-node list in section 7.

| Permission | Default | Covers |
| --- | --- | --- |
| `jpprison.user` | `true` | Every player command (`autosell`, `sell`, `sellall`, `balance`, `pay`, `top`, `blockstop`, `xptop`, `pets`, `armor`, `autominer`, `minion`, `robots`, `earn`, `stats`, `quests`, `boosts`, economy, `prestige`). |
| `jpprison.admin` | op | Every admin command: `/jpprison` and `/xpboost`, `/guide`, `/pets give`, `/pets luckyblock`, `/<eco> give|remove` and the economy boost. |
| `jpprison.pickaxe.noconfirm` | `false` (not even ops have it) | Removes the double-Q confirmation when dropping the pickaxe. |
| `jpprison.pet.<id>` | Optional per pet (`permission:` field in `pets.yml`) | Needed to equip it; unlock items and `/pets give` bypass it. In the bundled defaults every pet asks for it except `boomber` (the starter). |
| `jpprison.pet.armor` | From `menus/pet-armor.yml` (`permission:`, editable) | Gate to open the pet armor customization. |

## 1. Player commands (`jpprison.user`)

### `/prison`

With no arguments or `help` it shows the list of player commands.

**Subcommands:** `help` | `balance` | `prestige`

### `/prison balance`

Summary of all your economies, estimated income per hour (according to `autosell.yml` and `mining.estimated-blocks-per-minute`) and your 5 cheapest enchant upgrades (current level -> next + cost).

### `/prison prestige`

Global pickaxe prestige (same effect as `/jpprison prestige` or the button in the prestige menu): resets pickaxe XP (never enchants or economies) and raises prestige. It can cost an economy (`pickaxe.prestige.levels.<n>.cost`).

**Messages:** `pickaxe.prestige.*`

### `/autosell` (alias `/as`)

Toggles automatic selling. With autosell ON, blocks mined inside the `jpprison-enchants` flag are sold instantly instead of dropping; outside the flag they fall into your inventory even while the toggle is ON.

**Response:** `autosell.toggled-on` / `autosell.toggled-off`

### `/sell`

Sells the item you are holding. With no sellable items: `autosell.sell-all-none`.

### `/sellall` (alias `/sa`)

Sells the whole inventory. Message `autosell.sell-all` with `{amount}` and `{name}`.

### `/<economy>`

Balance of that economy. E.g.: `/tokens`, `/gems`, `/keys`. Format per its `economies/<id>.yml` (symbol + decimals).

### `/<economy> pay <player> <amount>`

Sends balance to another player (online or offline with `hasPlayedBefore`). Requires `allow-transfers: true` in that economy. You cannot pay yourself.

**Errors:** `economy.transfer-disallowed` / `transfer-self` / `insufficient` / `invalid-amount` / `transfer-cancelled` (if an event cancels the movement).

### `/<economy> top`

Top 10 of that economy (includes offline players, direct SQL).

### `/tokens` / `/gems` (alias `/token`)

`plugin.yml` shortcuts for the `tokens` and `gems` economies. They are equivalent to a bare `/tokens` and `/gems` (balance). Any new economy creates its own `/<id>` automatically: no need to touch `plugin.yml` or recompile.

### `/blockstop`

Top 10 of blocks mined.

### `/xptop`

Top 10 of pickaxe XP (includes offline).

### `/pets` (aliases `/pet`, `/pet-pets`)

Opens the pet menu (picker). Click on a pet: submenu (equip, customize armor, animations, particles).

`/pets help` explains unlock items, levels and boosts.

### `/armor` (alias `/armors`)

Opens the boost armor menu (equip/remove sets, view bonuses). Equipping places the 4 real pieces; removing them is only possible from the menu.

### `/autominer` (aliases `/autominers`, `/miner`)

Opens your autominer clone menu: START/STOP, status and stored time, and the UPGRADES button.

### `/minion` (aliases `/minions`, `/crew`)

Opens your crew menu: every type with its time, deploy/park, the PARK ALL button and a per-mob submenu (feed, upgrades).

### `/robots` (alias `/robot`)

Opens your robot fleet menu: every robot you have been given accumulates its economy day and night (even offline) up to the cap. Click a robot to collect ALL its stored economies, or the COLLECT ALL button to empty them all. Robots you do not own yet appear as a locked card.

A robot can pay SEVERAL economies at once (`income:` in its `.yml`), each with its own `amount-per-second` and its own storage; the lore shows one row per economy.

Robots level up WITH USE: every collection grants them XP equal to the seconds of production collected. Every level raises their income (and their capacity, which always holds the same hours of production). Per-robot cap in `robots/*.yml` (`max-level:`) or global in `robots.yml` (`leveling:`).

### `/earn` (alias `/earns`)

Opens the earnings dashboard. It explains itself: the bottom-center card ("HOW TO READ THIS") always tells you where you are and the 3 steps:

1. Top row: choose the period (8 windows).
2. Hopper: choose WHAT to measure (all / one economy / blocks / XP).
3. Click any card: its details.

Views (left rail, the active one glints):

- **SUMMARY** - your best source first, then one card per economy, blocks and XP. Click a card = its ranking.
- **RANKING** - who pays you the most, from highest to lowest: enchants AND systems (robots, minions, clone, bombs, drills, manual sales) with a % bar and an arrow vs the previous period.
- **HISTORY** - every period with its total and direction; click uses that period.
- **DETAILS** - (click a row) breakdown of that source by economy and by period, with session activations. The back button switches to "Back to ranking".

Bottom: session clock (time, sources paying, total, per hour) and page. The whole look in `menus/earn.yml`.

### `/stats [player]` (alias `/profile`)

Opens a profile card: the player's head (viewer or `[player]`, must be ONLINE) and one card per domain, everything read live from the DB: blocks + rank + milestone progress, pickaxe (level/XP/bar/prestige), ALL economies in rows, enchant summary, collection (pets, robots, armor), leaderboard ranks (blocks, XP and per economy) and your current MULTIPLIERS (same numbers as `/boosts`; one rewards row per economy).

Playtime comes from the vanilla `PLAY_ONE_TICK` statistic. Look and slots in `menus/stats.yml`; nothing else to configure.

### `/quests` (alias `/quest`)

Opens the daily and weekly quest board (`quests.yml`). Every card shows progress, bar, reward and countdown to the reset; when completed it is claimed with CLICK (the completed one glints). The summary block at the top shows how many are ready and the upcoming resets.

Types (fixed catalog, a new one needs code): `mine-blocks`, `mine-block` (specific material), `earn` (income of one economy), `collect-robot`, `prestige` and `open-crate`. Schedule and sound in `quests.yml` (`daily-reset-hour`, `weekly-reset-day/hour`); expired progress resets on its own.

### `/boosts` (alias `/multipliers`)

Shows in chat your current payout multipliers, one per line: pickaxe XP, autosell, momentum, drops, key chance, enchant luck and upgrade cost, plus one rewards row per economy. Respects `boosts.global-cap` (marks `(cap)`).

The same numbers appear in the `/stats` card and in the pickaxe menu head (`{boost-xp}` `{boost-sell}` `{boost-momentum}` `{boost-drops}` `{boost-key}` `{boost-lucky}` `{boost-cost}` `{boost-rewards}` in `menus/pickaxe.yml`). The per-SOURCE breakdown (prestige, xpboost, shards, runes, pet, combo, economy boost) still lives in the admin command `/jpprison boost <player>`. Chat templates in `messages.yml` (`boosts.header/line/cap/footer`).

### `/guide` (alias `/features`)

**Admin.** Developer reference hub (files, commands, API, PAPI). Requires `jpprison.admin`.

## 2. Admin commands (`/jpprison`, alias `/jp`)

The whole `/jpprison` command requires `jpprison.admin`. The `/<economy>` ones use `jpprison.user`; `give`, `remove` and the economy boost additionally ask for `jpprison.admin` inside the handler. With no arguments or `help` it shows the list (`usage.jpprison`).

### `/jpprison help`

Admin help list.

### `/jpprison reload`

Reloads config, messages, economies, enchants, menus, autosell, colors, orbs, remnants, bombs, drills, autominer, minions, robots, luckyblocks, loot chests, crates, shards, runes, pets and armors from disk. The `features:` flags apply instantly.

### `/jpprison backup`

Creates an immediate backup (`backups/backup-<timestamp>.zip`). Requires `features.backups`.

### `/jpprison announce <message...>`

Broadcast to the whole server. Accepts `&` and hex codes.

### `/jpprison balance`

Admin version of `/prison balance` (same economy summary, estimated incomes and next upgrades).

### `/jpprison prestige`

Admin version of the global prestige (same path as `/prison prestige`).

### `/jpprison xp <player> <amount>`

Gives raw pickaxe XP (no multipliers, no level-up notice), capped by `pickaxe.max-level`. ONLINE players only (exact name). `amount` > 0.

### `/jpprison boost <player>`

Read-only inspection of the player's final multipliers: XP, rewards per economy, autosell, momentum, drops, key, chance and upgrade cost, with every source (prestige, xpboost, shards, runes, pet, combo, economy boost). Respects `boosts.global-cap` and marks `(cap)`.

### `/jpprison economy`

Opens the economy creator (in-menu wizard). When finished it writes `economies/<id>.yml` and reloads without restarting. In-game only.

### `/xpboost <player|global> <mult> <time>`

**Admin.** Temporary pickaxe XP boost. `mult` is clamped to `1.01..100` and `time` to `pickaxe.xp-boosts.max-duration-minutes` (240 by default).

Valid times: `30s` | `2m` | `2:30m` (minutes:seconds). Examples:

```
/xpboost Steve 2 10m
/xpboost global 1.5 30s
```

Notice to those affected + sound; the admin receives `xpboost.given`.

### `/<economy> give <player> <amount>`

**Admin.** Gives balance (accepts offline with `hasPlayedBefore`). `amount` > 0. Example: `/tokens give Steve 5000`

### `/<economy> remove <player> <amount>`

**Admin.** Removes balance (accepts offline; never goes below 0). Example: `/gems remove Steve 250`

### `/<economy> <player|global> <mult> <time>`

**Admin.** Temporary boost of that economy (same mechanic as `/xpboost`). `mult` `1.01..100`, `time` `30s` | `2m` | `2:30m`, cap `economy-boosts.max-duration-minutes` (240).

Example: `/tokens Steve 2 2m` | `/gems global 1.5 30s`

## 3. Item deliveries (admin)

Direct shortcuts for the boost items: `/orb`, `/remnant`, `/shard` and `/rune` (plus the plurals `/orbs`, `/remnants`, `/shards`, `/runes`). The long forms `/jpprison <type> give` still work, and a leading "give" is optional (e.g. `/shard give Steve power-shard 5` also works).

All of them ask for an ONLINE player with the exact name (except `/jpprison armor reset`, which accepts offline).

### `/orb <player> <xp> [count]`

Shortcut of `/jpprison orb give` (which still works). Orb items: applied to the pickaxe with a click in the inventory (the whole stack). Includes the pickaxe prestige multiplier. `count` defaults to 1. Item look in `orbs.yml`.

### `/remnant <player> <enchant> <levels> [count]`

Shortcut of `/jpprison remnant give` (which still works). Remnants: they give levels of THAT enchant to the pickaxe when clicked. Applying one requires the enchant's pickaxe level (`required-pickaxe-level`). `levels` > 0; `count` defaults to 1.

Enchant ids: the ones in `/enchants/*.yml` (`fortune`, `efficiency`, `token-finder`, `meteor`...). Example: `/remnant Steve fortune 5 3`

### `/jpprison bomb give <player> [bomb] [count]`

Alias `/jpprison bombs`. Throwable bombs (right-click in hand). If `[bomb]` is omitted the default bomb is used. The legacy syntax `/jpprison bomb give <player> <count>` still works. Bundled ids: `tnt`, `nuke`, `mini` (the ones in `bombs.yml`).

### `/jpprison bomb check`

**In-game.** Diagnostic at your feet: WorldGuard hook state, flag value (`ALLOW`/`DENY`/`undefined`), `require-flag` and verdict (`WOULD DETONATE` / `WOULD FIZZLE` + reason). Uses the same matrix as the real detonation.

### `/jpprison drill give <player> [drill] [count]`

Alias `/jpprison drills`. Placeable drills: right-click on a block to plant the hopper. The legacy syntax `/jpprison drill give <player> <count>` still works. Bundled ids: `drill`, `excavator`.

### `/jpprison autominer give <player> <time>`

Alias `/jpprison autominers`. Gives autominer clone time. Time format: `30s` | `10m` | `2:30m` | `2h` | `1d` | plain number (= minutes). Clamped to the `max-stored-seconds` cap (0 = no cap). Time is only charged per block broken.

### `/jpprison autominer check`

**In-game.** Diagnostic at your feet: hook, flag, stored time, state (running) and verdict with reason.

### `/jpprison minion give <player> <minion> <time> [count]`

Alias `/jpprison minions`. Minion time items (right-click in hand to feed THAT mob). Same time format. `count` 1..64 (default 1). Bundled ids: `zombie`, `pigman`, `villager`.

### `/jpprison minion check`

**In-game.** Diagnostic: hook, flag, how many you have out (x/max) and one line per known mob with `{minion}`: time (status).

### `/jpprison robot give <player> <robot>`

Alias `/jpprison robots`. Gives a permanent robot to the player (works with an offline player who has played before). Bundled ids: `gem-bot`, `hybrid-bot`, `money-bot`, `token-bot`. If they already have it, it warns and does not duplicate it.

### `/jpprison robot remove <player> <robot>`

Removes that robot (and its storage) from the player. If they did not have it, it warns.

### `/jpprison robot list <player>`

Lists the player's robots with what they have stored right now.

### `/jpprison luckyblock give <player> <block> [count] [ready]`

Alias `/jpprison luckyblocks`. Mining LuckyBlocks (`luckyblocks/` folder; bundled ids: `common`, `rare`, `legendary`). By default it is delivered SEALED (gray head with the block requirement); add the word "ready" as the last argument to deliver it already unlocked.

### `/shard <player> <shard> [count]`

Shortcut of `/jpprison shard give` (which still works). Pickaxe shards: inserted in the shards menu (opened from the pickaxe menu). Ids from `shards.yml` (`power-shard`, `xp-shard`, `rich-shard`, `momentum-shard`, `lucky-shard`, `miner-shard`, `harvester-shard`, `key-shard`, `savings-shard`). `count` defaults to 1.

### `/rune <player> <rune> [count]`

Shortcut of `/jpprison rune give` (which still works). Pickaxe runes: same flow in the runes menu. Ids from `runes.yml` (`power-rune`, `lucky-rune`, `fortune-rune`, `holy-rune`, `wisdom-rune`, `wealth-rune`, `fervor-rune`, `greed-rune`, `cheap-rune`).

### `/jpprison crate give <player> <crate> [count]`

Alias `/jpprison crates`. Crates: items opened with a right-click in hand. Consumes 1 and plays the opening animation in front of the player (the crate rises to the center, the loot orbits and one weighted prize is revealed). `count` defaults to 1. Bundled ids: `vote`, `common`, `legendary` (each file in `crates/*.yml` defines its id).

### `/jpprison crate open <crate>`

**In-game.** Tests that crate's opening animation without consuming an item (for configuring the file). Respects the "already opening" state.

### `/jpprison pet remove <player>`

Removes the player's active pet (cleanup).

### `/jpprison pet list`

Lists every defined pet id and name.

### `/jpprison armor reset <player> [armor]`

Alias `/jpprison armors`. Resets a player's armor data (level 1 / 0 XP) and re-equips the highest unlocked one. Accepts offline players. `[armor]` limits the reset to one specific set. Bundled ids: `wraps`, `crimson`, `amber`, `moss`, `azure`, `violet`, `chain`, `iron`, `gilded`, `diamond`.

### `/pets give <player> <pet>`

**Admin.** Gives the unlock ITEM for that pet (it does not summon it). The item is used with a right-click in hand and unlocks the pet forever. Bundled ids: `boomber`, `rotty`, `bonez`, `voidwalker`, `ember`, `webby`, `specter`, `hexa`, `grumble`, `venom`, `oinker`, `purrito`, `boing`.

### `/pets luckyblock give <player> [count]`

**Admin.** Gives pet LuckyBlocks (`count` 1..64, default 1). Right-click in hand: ~7s show and unlocks a random unowned pet (`luckyblock-weight` in `pets.yml`).

### `/pets luckyblock check`

**In-game.** LuckyBlock diagnostic: item in hand, show in progress, pool of remaining pets, menu session state and verdict.

## 4. Time, multiplier and amount formats

### Boost times

Used by `/xpboost` and `/<eco> <player|global> <mult> <time>`:

| Format | Meaning |
| --- | --- |
| `30s` | 30 seconds |
| `2m` | 2 minutes |
| `2:30m` | 2 minutes 30 seconds (minutes:seconds) |

They do not accept `h` (hours) or `d` (days). Multiplier: `1.01` to `100` (values outside are clamped to the limit). Maximum duration: `pickaxe.xp-boosts.max-duration-minutes` (XP) and `economy-boosts.max-duration-minutes` (economies), 240 by default.

### Autominer and minion times

Used by `/jpprison autominer|minion give ... <time>`: `30s` | `10m` | `2:30m` | `2h` | `1d` | `45` (plain number = minutes). They are clamped to `minions.yml`/`autominer.yml` `max-stored-seconds` (0 = no cap). Time is spent per block broken, not for walking/standing still.

### Robot storage (`/robots`)

It is not fed by hand. Every robot accumulates its economy (or economies, `robots/*.yml`) up to the `max-stored-seconds` cap of `robots.yml` (or the robot's own override; 0 = no cap) and is collected with a click in the menu. A robot can pay several economies at once with the `income:` map -- `income: {tokens: 10, gems: 1}` -- every economy has its own pace and its own independent storage (the cap is the same number of hours of production per economy). Collecting counts as income: an active `/<eco>` boost multiplies that economy and the recap records it.

Every collection raises the robot's LEVEL: XP = seconds of production collected x `leveling.xp-per-second-collected`; every level multiplies ALL its economies by `1 + (level-1) x leveling.income-per-level` and the capacities grow with them. Curve and bar in `robots.yml` `leveling:`; per-robot cap with `max-level:`.

### Amounts

- Economy `give`/`remove`/`pay`: decimal numbers > 0 (rounded to the display decimals when formatting; storage keeps full precision).
- Orb XP and `/jpprison xp`: integers > 0.
- Remnant levels: integers > 0.
- Item `count`: integers > 0; minion and luckyblock are clamped to 1..64.
- Player names: the item gives and `/jpprison xp` ask for an ONLINE player (exact name); economy `give`/`remove`/`pay`, `armor reset` and `/jpprison robot give|remove|list` accept an offline player who has played before.

### Mining milestones (`milestones.yml`)

UNIQUE rewards when reaching X total blocks mined (the `blocks_mined` counter of `/blockstop`). Every source counts: manual mining, enchant sweeps, autominer clone and minions. Crossing the threshold: console commands + sound + chat line (or the milestone's own `message:`) + optional broadcast. Thresholds already passed are marked silently on join (`retroactive: false`) so only new crossings pay; `retroactive: true` pays the backlog on the next block. No command: edit `milestones.yml` + `/jpprison reload`.

### Daily and weekly quests (`quests.yml`)

Goals with automatic progress and a reward claimed by hand from `/quests`. Types: `mine-blocks` (any block), `mine-block` (`block: DIAMOND_ORE`), `earn` (`economy:` + real income, `give`/`pay` do not count), `collect-robot` (`economy:` optional; with it, it counts units of that economy, without it it counts collections), `prestige` and `open-crate`. Global config: `daily-reset-hour` (0-23), `weekly-reset-day` (`MONDAY..SUNDAY`) and `weekly-reset-hour`, and `sound`. Every entry: `type`, `amount`, `reset` (`daily|weekly`), `name`, `description`, `icon`, `commands` (console with `{player}`), `reward` (summary for the chat/menu) and optional `broadcast`. Progress lives in one row per player+quest (period, progress, claimed): when the period changes it resets on its own, and the claimed flag is written instantly (a crash cannot pay twice). Pruned on reset: unclaimed progress is lost with the period. No admin command. The bundles use `daily-miner`/`daily-diamonds`/`daily-income`/`daily-robots` + `weekly-marathon`/`weekly-prestige`/`weekly-crates`; edit them and `/jpprison reload`. If a feature is `features: false` it is rejected all the same.

## 5. Items that are not commands (right-click / inventory click)

### XP orbs (`orbs.yml`)

Click the orb onto the pickaxe (or the pickaxe onto the orb) in any inventory. Consumes the whole stack, applies `xp*count` with the pickaxe prestige multiplier and refreshes the lore. With the pickaxe at the cap (`pickaxe.max-level`) it is rejected with `orbs.max-level`.

### Remnants (`remnants.yml`)

Same gesture with the pickaxe; they give levels of the indicated enchant (capped by prestige). If the enchant is locked by pickaxe level: `remnants.locked`, it is not consumed.

### Pet unlock items (`pets.yml` `give:`)

Right-click in hand, with no menu open. Consumes 1 and unlocks the pet. If you already have it, it reopens the picker without consuming.

### Pet LuckyBlock (`pets.yml` `luckyblock:`)

Right-click in hand, with no menu open. Consumption + show; with an empty pool or a show in progress it spends nothing.

### Mining LuckyBlocks (`luckyblocks/`)

They drop into the inventory while mining (rare, according to each type's `chance`, in file order; requires the `jpprison-enchants` flag ALLOW like the loot chests). They arrive SEALED: a gray head whose lore asks you to mine `{required}` blocks. Every block you break with one in the inventory raises its `{progress}` (stored on the item itself, no DB: it survives trading or storing) and on completion the texture changes to a lucky block with sound, title (can be muted with `unlock-title: false`) and message. Right-click on an unlocked one: consumes 1 and resolves its `rewards`: bundles with `weight:` enter a roulette (exactly 1 comes out per opening, with relative odds), those with `chance:` roll separately and can combine; every winner runs its console commands and its `display:` is summarized in chat. Optional broadcast.

### Crates (`crates/*.yml`)

Right-click in hand, with no menu open. Consumes 1 and plays the animation: the crate rises to the center (3 blocks by default), the configured loot orbits around it and the roulette reveals a winner weighted by its `weight`/`chance`. Prizes run as console commands with `{player}`; every entry can carry its own `message`, `broadcast` and `sound`. If there is no free ground ahead: `crates.no-space` and the item is NOT consumed. An interrupted show (quit/reload/stop) still pays the drawn prize.

### Bombs (`bombs.yml`)

Right-click in hand to throw toward where you aim. They explode on arrival, on hitting a wall or by timeout. They require the `jpprison-enchants` flag (ALLOW) where they break; if they break nothing the bomb is refunded (`bombs.refunded`).

### Drills (`drills.yml`)

Right-click on the TOP FACE of a block (the hopper is planted there). It bores a vertical shaft and pays per block broken. Breaking the hopper stops it with a partial payout, no refund.

### Minion time items (`minions.yml` `time-item:`)

Right-click in hand to feed that specific mob. The FEED button of the submenu consumes the items that fit directly from the inventory.

### Shards and runes

They are not used with a right-click; they are inserted by clicking a slot in the shards/runes menu (opened from the pickaxe menu, slots 6 and 5). Normal click inserts/upgrades, right-click or shift extracts.

### Other protected gestures

Pickaxes (indestructible, double-Q confirmation with `pickaxe.drop-confirm`), worn armors (they cannot be removed outside the `/armor` menu) and NPCs (autominer/minions/bombs/pets cannot be damaged/rotated/stolen).

## 6. Common errors

- **"Unknown command"**: misspelled economy/enchant or its `.yml` does not exist (ids are lowercase; `token-finder`, `frost-nova` etc. use hyphens).
- **"No permission"**: you lack `jpprison.admin` (admin commands) or the permission of a specific pet. To drop the pickaxe without the double Q you must have `jpprison.pickaxe.noconfirm` (default false).
- **Player not found**: the item gives and `/jpprison xp` require the player ONLINE; economy `give`/`remove`/`pay` and `/jpprison armor reset` do accept offline.
- **"That transfer was blocked"**: a `BalanceChangeEvent` (another plugin) cancelled the movement; in `pay` the sender is refunded.
- **The boost changes nothing**: check the multiplier (`1.01..100`) and the time format (`30s` | `2m` | `2:30m`), and that it is not above the maximum duration.
- **The command "does nothing"**: the feature is `features: false`. Review the `features:` section of `config.yml` and run `/jpprison reload`.
- **`/jpprison` asks for `jpprison.admin` even for `help`**: normal players use `/prison help`, `/prison balance` and `/prison prestige`.
- **`/<economy>` with arguments only understands `give`, `remove`, `pay` and `top`** (plus `<player|global> <mult> <time>` for admin). Any other argument falls back to the player balance (e.g. `/tokens foo` shows your balance).
- **Tab-complete does not suggest an id**: the system did not load (check the `.yml` and the console warnings), or the name has spaces (pets accept the clean name, without the glyph or colors).

## 7. Full permission list

Declared in `plugin.yml` (they show up in `/permissions` or in the permission manager):

### `jpprison.user` -- default: `true`

Base of every player command and of `/<economy>` (tokens, gems...). Covers: `/prison`, `/autosell`, `/sell`, `/sellall`, `/blockstop`, `/xptop`, `/pets`, `/armor`, `/autominer`, `/minion`, `/robots`, `/earn`, `/stats`, `/quests`, `/boosts`, `/<eco>` (balance, pay and top) and their tab-completes. The per-pet nodes are separate (see below).

### `jpprison.admin` -- default: op

Every admin command: `/jpprison` (the whole thing), `/xpboost`, `/guide` (alias `/features`), `/pets give`, `/pets luckyblock give|check`, `/<eco> give|remove` and the economy boost (`/<eco> <player|global> <mult> <time>`).

### `jpprison.pickaxe.noconfirm` -- default: `false` (not even ops have it)

Removes the double-Q confirmation when dropping the pickaxe (`PickaxeGuard`). Without it: the first Q is cancelled and the second one within `pickaxe.drop-confirm-seconds` drops it.

**Data permissions** (not in `plugin.yml`; each module reads them from its `.yml`):

### `jpprison.pet.armor`

`menus/pet-armor.yml` -> `permission:`. Gate to open the pet armor customization (from the submenu or its own menu). Empty in the yml = no gate.

### `crates/*.yml` -> `permission:`

Gate to OPEN a specific crate (e.g. `permission: "jpprison.crate.vip"`). Empty/absent = anyone can open it. The admin give is not gated.

### `jpprison.pet.<id>`

`pets.yml` -> `pets.<id>.permission:`. Gate to equip THAT pet from the picker/submenu. In the bundle every pet asks for it except `boomber` (the starter):

```
jpprison.pet.rotty
jpprison.pet.bonez
jpprison.pet.voidwalker
jpprison.pet.ember
jpprison.pet.webby
jpprison.pet.specter
jpprison.pet.hexa
jpprison.pet.grumble
jpprison.pet.venom
jpprison.pet.oinker
jpprison.pet.purrito
jpprison.pet.boing
```

The unlock items (given with `/pets give`, right-click in hand) bypass the permission: they stay unlocked forever. Deleting or emptying the pet's `permission:` field = no gate. Every new pet uses the permission you put in its `.yml` (or none).

**No per-economy, per-enchant or per-command nodes exist:**

- Fine control is done with `features:` in `config.yml`: with a feature disabled its commands are rejected even if you have `jpprison.admin`.
- Your permission manager's panel will only list the 3 declared in `plugin.yml`; the data ones (pets/armor) are granted the same way but must be written by hand in LuckPerms/GroupManager/etc.

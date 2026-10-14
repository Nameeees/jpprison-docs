# Placeholders

JPPrison - All PlaceholderAPI variables.

- **Expansion:** `%jpprison_<variable>%`
- **Requirement:** PlaceholderAPI installed on the server. If PlaceholderAPI is not installed, placeholders are shown as literal text.
- **Null player or unknown placeholder:** returns `""` (empty).
- **Every placeholder is read-only** (none of them modify anything).

General format:

```
%jpprison_blocks_mined%
^^^^^^^^ ^^^^^^^^^^^^^
|        |
|        +- variable name (see the list below)
+- fixed identifier of the expansion (always "jpprison")
```

Where to use them: scoreboards (FeatherBoard, TAB, etc.), chat (VentureChat), holograms (DecentHolograms/HolographicDisplays), menus, MOTD, tablist, etc.

Example in another plugin's config:

```
- "&7Blocks: &e%jpprison_blocks_mined_formatted%"
- "&7Tokens: &b%jpprison_tokens%"
```

**NOTE about abbreviated numbers (`_formatted` / `_raw`):**

- `_raw` and `_formatted` values use short format: `1.25K`, `3.4M`, `2B`, etc.
  Examples: `999` -> `"999"`, `1500` -> `"1.5K"`, `2500000` -> `"2.5M"`, `1234567890` -> `"1.23B"`
- Values without a suffix (e.g. `blocks_mined`, `pickaxe_xp`) are the exact number.
- Balances with economy formatting (e.g. `%jpprison_tokens%`) use `Economy.format()`: symbol + decimals according to `economies/<id>.yml`.

## 1. Player stats (mining / pickaxe)

### `%jpprison_blocks_mined%`

Total blocks mined (exact number).

- **Example:** `15234`
- **Use:** `You have mined %jpprison_blocks_mined% blocks`

### `%jpprison_blocks_mined_formatted%`

Same value, abbreviated.

- **Example:** `15234` -> `"15.23K"`
- **Use:** scoreboard `Blocks: %jpprison_blocks_mined_formatted%`

### `%jpprison_pickaxe_level%`

Current pickaxe level.

- **Example:** `27`
- **Use:** `&7Level: &d%jpprison_pickaxe_level%`

### `%jpprison_pickaxe_xp%`

Current accumulated pickaxe XP (exact).

- **Example:** `89350`

### `%jpprison_pickaxe_xp_formatted%`

Pickaxe XP, abbreviated.

- **Example:** `89350` -> `"89.35K"`

### `%jpprison_pickaxe_xp_needed%`

Total XP required for the NEXT level (exact).

- **Example:** `102400`
- **Use:** `You are missing %jpprison_pickaxe_xp_needed% XP` (it is the threshold, not the remaining amount; for the remaining amount subtract `xp_needed - xp`).

### `%jpprison_pickaxe_xp_needed_formatted%`

Same value, abbreviated.

- **Example:** `"102.4K"`

### `%jpprison_pickaxe_progress%`

Progress bar toward the next level.

- **Example (with colors):** `"||||||||||||||||||||"`
- **Use:** `%jpprison_pickaxe_progress% %jpprison_pickaxe_progress_percent%`
- **Note:** respects `pickaxe.progress-bar.length/filled/empty` from `config.yml` and the player's theme colors. At max level (`pickaxe.max-level`) it shows the full bar (100%).

### `%jpprison_pickaxe_progress_percent%`

Progress percentage.

- **Example:** `"63%"`
- At max level it returns `"100%"`.

### `%jpprison_pickaxe_color%`

ID of the pickaxe theme/color (e.g. `"default"`, `"red"`).

- It is the value stored in the DB (`getLoreColor`).

### `%jpprison_pickaxe_color_name%`

Visible theme name (with colors translated, without `&`).

- **Example:** if the theme is called `"&cRed"`, it returns `"Red"` in red.
- **Use:** `Theme: %jpprison_pickaxe_color_name%`

### `%jpprison_pickaxe_prestige%`

GLOBAL prestige level of the pickaxe.

- **Example:** `3`
- **Use:** `Prestige: %jpprison_pickaxe_prestige%`

## 2. Economies (dynamic: works for ANY `/economies/*.yml` id)

By default these exist: `tokens`, `gems` (+ `money` if Vault is present).
If you create `economies/keys.yml`, then `%jpprison_keys%` etc. automatically exist.
The id is lowercase (`Tokens` = `tokens`).

For each `<eco>` (example with `tokens`, identical for `gems`, `money`, `keys`...):

### `%jpprison_tokens%`

Formatted balance (symbol + decimals according to its `.yml`).

- **Example:** `"1,250 Tokens"` (depends on symbol/name/decimals)
- **Use:** `You have %jpprison_tokens%`

### `%jpprison_tokens_formatted%`

Exact alias of the previous one. Same value.

### `%jpprison_tokens_raw%`

Abbreviated balance, without symbol.

- **Example:** `1250000` -> `"1.25M"`
- **Use:** compact scoreboard.

### `%jpprison_tokens_symbol%`

Economy symbol (the `symbol` field of the yml).

- **Example:** `"$"`, `"T"`, etc.

### `%jpprison_tokens_name%`

Display name (the `name` field of the yml).

- **Example:** `"Tokens"`, `"Gems"`

### `%jpprison_tokens_boost_active%`

`"true"` only while an economy boost is active and paying above x1 for THAT player and THAT economy. If there is no boost, or it already expired: `"false"`.

- **Use with conditionals:** show the boost line only if it is true.
- **Activated with:** `/tokens <player|global> <mult> <time>`
- **Example:** `/tokens Steve 2 10m` -> x2 for 10 min.

Complete examples:

```
"Tokens: %jpprison_tokens% (%jpprison_tokens_raw%)"
"Gems: %jpprison_gems% | Active x2: %jpprison_gems_boost_active%"
"Money: %jpprison_money%"
```

**VAULT SPECIAL CASE** (money from other plugins: Essentials, CMI...):

### `%jpprison_vault_name%`

Vault currency name.

- **Example:** `"Money"`
- If Vault is not present: `"Money"`.

### `%jpprison_vault_money%`

Formatted Vault balance. Alias: `%jpprison_vault_money_formatted%`.

- If there is no Vault: `"0"`.

### `%jpprison_vault_money_raw%`

Abbreviated Vault balance (`1.5K`/`2M`...). Without Vault: `"0"`.

- **Use:** `Money: %jpprison_vault_money%`

### `%jpprison_autosell%`

`"true"` if the player has `/autosell` enabled, `"false"` if not.

- **Use:** `Auto-sell: %jpprison_autosell%`

## 3. Enchants (dynamic: works for ANY `/enchants/*.yml` id)

Bundled enchants (35):

```
efficiency, fortune, speed, haste, night-vision, jump,
token-finder, gem-finder, xp-finder, lucky, greed, greed-money, overflow,
blessed, key-finder, key-all, token-merchant,
explosive, jackhammer, layer, nuke, thunder, meteor, cross, solar,
frost-nova, geyser, soul-spiral, comet, volcano, blackhole, aurora,
sandstorm, apocalypse, momentum
```

For each `<id>` these 12 variables exist (example with `fortune`):

### `%jpprison_fortune_level%`

Current level of the player.

- **Example:** `12` (0 = not purchased)

### `%jpprison_fortune_max_level%`

Base max level from the yml (without prestige).

- **Example:** `50`

### `%jpprison_fortune_effective_max_level%`

Real max level with prestige (`max-level-bonus`).

- **Example:** `55`

### `%jpprison_fortune_prestige%`

Prestige of THAT enchant.

- **Example:** `2`

### `%jpprison_fortune_activations%`

Times it has activated (counter).

- **Example:** `340`

### `%jpprison_fortune_disabled%`

`"true"` if the player disabled it, `"false"` if enabled.

### `%jpprison_fortune_virtual%`

`"true"` if it is in virtual mode (does not break blocks), `"false"` for normal.

### `%jpprison_fortune_muted%`

`"true"` if its messages are muted, `"false"` if they play.

### `%jpprison_fortune_chance%`

REAL chance at the current level (with prestige).

- **Example:** `"12.5"` (a number with 1 decimal, no `%`; add the `%` yourself in the text)
- **Use:** `%jpprison_lucky_chance%% Lucky`

### `%jpprison_fortune_next_cost%`

Cost of the NEXT level (exact number, with shard/`cost` discount and prestige already applied). `0` if it is at max.

- **Example:** `13000`
- **Use:** `Next: %jpprison_fortune_next_cost_formatted%`

### `%jpprison_fortune_next_cost_formatted%`

Same value, abbreviated.

- **Example:** `"13K"`

### `%jpprison_fortune_upgrade_economy%`

Economy used to upgrade it (`upgrade-economy` from the yml).

- **Example:** `"tokens"` or `"gems"` (destructive enchants are usually gems, normal ones tokens)

Complete examples:

```
"Fortune: %jpprison_fortune_level%/%jpprison_fortune_max_level% (real max %jpprison_fortune_effective_max_level%)"
"Explosive: level %jpprison_explosive_level% | chance %jpprison_explosive_chance%%% | prestige %jpprison_explosive_prestige%"
"Upgrading Lucky costs %jpprison_lucky_next_cost_formatted% %jpprison_lucky_upgrade_economy%"
"Token Finder disabled: %jpprison_token-finder_disabled%"  (careful: the id has a hyphen, write it as-is)
```

## 4. Enchant summary (global counters)

### `%jpprison_enchant_count%`

How many enchants you have at level > 0.

- **Example:** `8`
- **Use:** `Enchants: %jpprison_enchant_count%`

### `%jpprison_total_levels%`

Sum of ALL levels.

- **Example:** `145`
- **Use:** `Total levels: %jpprison_total_levels%`

### `%jpprison_maxed_count%`

How many enchants are at the base max.

- **Example:** `2`
- **Use:** `Maxed: %jpprison_maxed_count%/35`

## 5. Momentum (mining combo)

### `%jpprison_momentum_multiplier%`

Current multiplier with `x` in front.

- **Example:** `"x2.50"`
- Read-only (reading it does not extend the combo).

### `%jpprison_momentum_multiplier_raw%`

Same value, without the `x`.

- **Example:** `"2.50"`
- Useful for calculations in other plugins.

### `%jpprison_momentum_active%`

`"true"` if the combo is still alive, `"false"` if it expired.

- **Use:** show/hide a line: `Combo: %jpprison_momentum_multiplier%`

Scoreboard example:

```
- "&7Combo: &e%jpprison_momentum_multiplier%"
- "&7Active: &f%jpprison_momentum_active%"
```

## 6. XP boost (temporary pickaxe XP boost)

Activated with: `/xpboost <player|global> <mult> <time>`

Example: `/xpboost Steve 2 5m` | `/xpboost global 1.5 30s`

### `%jpprison_xpboost_multiplier%`

Multiplier with `x`.

- **Example:** `"x2.00"` (without boost: `"x1.00"`)

### `%jpprison_xpboost_remaining%`

Remaining minutes (whole number).

- **Example:** `4`

### `%jpprison_xpboost_active%`

`"true"` ONLY while it pays above x1. Use it to hide the line when there is no boost (it never shows a dead `"x1.00"`).

Example:

```
- "&7XP Boost: &b%jpprison_xpboost_multiplier% &8(%jpprison_xpboost_remaining% min)"
```

## 7. Armors (`/armor` sets)

### `%jpprison_armor%`

ID of the equipped set, `""` if none.

- **Example:** `"diamond"`
- **Use:** `Set: %jpprison_armor_name%`

### `%jpprison_armor_name%`

Visible name, without colors. If there is no set: `""`.

- **Example:** `"Diamond Guard"`

### `%jpprison_armor_level%`

Level of the equipped set.

- **Example:** `7` (0 if none)

### `%jpprison_armor_count%`

How many sets you have unlocked.

- **Example:** `3`

Example:

```
"Armor: %jpprison_armor_name% Lv.%jpprison_armor_level% (%jpprison_armor_count% unlocked)"
```

## 8. Autominer (mining clone)

### `%jpprison_autominer_active%`

`"true"` if your clone is mining, `"false"` if it is stopped.

### `%jpprison_autominer_time%`

Stored time, formatted.

- **Example:** `"1h 2m"`, `"45s"`, `"0s"`

### `%jpprison_autominer_time_seconds%`

Same value, in exact seconds. Useful to compare/sort.

- **Example:** `3720`

Example:

```
"Miner: %jpprison_autominer_active% | Time: %jpprison_autominer_time%"
```

## 9. Minions (mini-mobs, ids from `/minions/*.yml`: zombie, pigman, villager)

### `%jpprison_minions_active%`

How many mobs you have out mining.

- **Example:** `2` (max 3)

For each `<id>` (example with `zombie`, identical for pigman/villager):

### `%jpprison_minion_active_zombie%`

`"true"` if THAT mob is out, `"false"` if it is stored.

### `%jpprison_minion_time_zombie%`

Stored time, formatted.

- **Example:** `"30m"`, `"45s"`

### `%jpprison_minion_time_seconds_zombie%`

Time in exact seconds.

- **Example:** `1800`

Examples:

```
"Minions out: %jpprison_minions_active%/3"
"Zombie: %jpprison_minion_time_zombie% (%jpprison_minion_active_zombie%)"
```

## 10. Robots (passive workers, ids from `/robots/*.yml`: gem-bot, money-bot, token-bot)

### `%jpprison_robots_owned%`

How many robots you have (0 if none).

- **Example:** `2`

### `%jpprison_robots_ready%`

How many have something ready to collect.

- **Example:** `1`

For each `<id>` (example with `token-bot`, identical for gem-bot/money-bot/hybrid-bot). Placeholders WITHOUT a suffix read the FIRST payout of the robot:

### `%jpprison_robot_owned_token-bot%`

`"true"` if you have THAT robot, `"false"` if not.

### `%jpprison_robot_stored_token-bot%`

Stored right now (it can grow between queries).

- **Example:** `12.5K`

### `%jpprison_robot_cap_token-bot%`

Storage cap, level-scaled (0 = no cap).

- **Example:** `345.6K`

### `%jpprison_robot_rate_token-bot%`

Production per second ALREADY with the level bonus applied.

- **Example:** `12`

### `%jpprison_robot_economy_token-bot%`

Id of the first economy it pays.

- **Example:** `"tokens"`

### `%jpprison_robot_level_token-bot%`

Current level (1 = base).

- **Example:** `3`

### `%jpprison_robot_xp_token-bot%`

XP inside the current level.

- **Example:** `850`

### `%jpprison_robot_xp_needed_token-bot%`

XP for the next level (0 if it is at the cap).

- **Example:** `2500`

### `%jpprison_robot_max_level_token-bot%`

Level cap of that robot.

- **Example:** `25`

### `%jpprison_robot_bonus_token-bot%`

Income bonus per level, in % (plain number; add the `%` yourself).

- **Example:** `10`

**MULTI-ECONOMY ROBOTS** (`income:` in `robots/*.yml`): one suffix per economy (example with `hybrid-bot`, which pays tokens and gems):

### `%jpprison_robot_stored_hybrid-bot_tokens%`

Stored amount of THAT economy.

- **Example:** `1.25K`

### `%jpprison_robot_rate_hybrid-bot_gems%`

Rate of THAT economy (with level).

- **Example:** `0.28`

### `%jpprison_robot_cap_hybrid-bot_gems%`

Cap of THAT economy.

- **Example:** `7.2K`

Examples:

```
"Robots: %jpprison_robots_owned% | Ready: %jpprison_robots_ready%"
"Token Bot: %jpprison_robot_stored_token-bot% / %jpprison_robot_cap_token-bot%"
"Token Bot Lv.%jpprison_robot_level_token-bot% (+%jpprison_robot_bonus_token-bot%%)"
"Hybrid Bot: %jpprison_robot_stored_hybrid-bot_tokens% tokens, %jpprison_robot_stored_hybrid-bot_gems% gems"
```

## 11. Ranks (your position - includes offline players, via direct SQL)

They return a position number starting at 1. If you have no data, your position still counts.

### `%jpprison_top_blocks%`

Your place in blocks mined.

- **Example:** `5`

### `%jpprison_top_xp%`

Your place in pickaxe XP.

- **Example:** `12`

### `%jpprison_top_tokens%`

Your place in tokens. (works for ANY eco: `top_gems`, `top_money`, `top_keys`...)

- **Example:** `%jpprison_top_gems%`

Example:

```
"You are # %jpprison_top_blocks% in blocks and #%jpprison_top_tokens% in tokens"
```

## 12. Server tops (name + value, positions 1-10, 30s cache)

Format: `%jpprison_top_<what>_<n>_name%` and `%jpprison_top_<what>_<n>_value%`

### `%jpprison_top_<what>_<n>_name%`

Player name of position `<n>` (or the truncated UUID if it cannot be resolved).

### `%jpprison_top_<what>_<n>_value%`

Formatted value of position `<n>` (abbreviated for blocks/xp, `Economy.format` for economies).

Notes:

- `<what>` = `blocks` | `xp` | `<eco-id>` (`tokens`, `gems`, `money`, ...)
- `<n>` = 1 to 10 (out of range returns empty)
- Updates every 30 seconds (cache). If there is no data it returns empty.

Examples:

```
"#1 Blocks: %jpprison_top_blocks_1_name% (%jpprison_top_blocks_1_value%)"
"#1 XP: %jpprison_top_xp_1_name% (%jpprison_top_xp_1_value%)"
"#1 Tokens: %jpprison_top_tokens_1_name% (%jpprison_top_tokens_1_value%)"
"#2 Tokens: %jpprison_top_tokens_2_name% (%jpprison_top_tokens_2_value%)"
"#3 Gems: %jpprison_top_gems_3_name% (%jpprison_top_gems_3_value%)"
```

Ready-to-paste hologram table for the top tokens (copy/paste):

```
&6&lTOP TOKENS
&e#1 &f%jpprison_top_tokens_1_name% &7- &b%jpprison_top_tokens_1_value%
&e#2 &f%jpprison_top_tokens_2_name% &7- &b%jpprison_top_tokens_2_value%
&e#3 &f%jpprison_top_tokens_3_name% &7- &b%jpprison_top_tokens_3_value%
&e#4 &f%jpprison_top_tokens_4_name% &7- &b%jpprison_top_tokens_4_value%
&e#5 &f%jpprison_top_tokens_5_name% &7- &b%jpprison_top_tokens_5_value%
```

## 13. Quests (daily and weekly quests, ids from `quests.yml`)

For each `<id>` of the `quests:` section of `quests.yml` (the bundled ones: `daily-miner`, `daily-diamonds`, `daily-income`, `daily-robots`, `weekly-marathon`, `weekly-prestige`, `weekly-crates`):

### `%jpprison_quest_daily-miner_progress%`

Current progress, formatted (abbreviated).

- **Example:** `"640"` / `"12.5K"`

### `%jpprison_quest_daily-miner_goal%`

Quest goal, formatted.

- **Example:** `"1K"`

### `%jpprison_quest_daily-miner_claimable%`

`"true"` if it is complete and NOT claimed (exactly the one the player should claim).

### `%jpprison_quest_daily-miner_claimed%`

`"true"` if it was already claimed this period.

Notes:

- When the period changes (daily/weekly reset), progress goes back to 0 by itself.
- Use them for "pending quests" scoreboards: sum the claimable of several quests.
- Ids keep their hyphen when they have one (`daily-miner`), written as-is.

Example (quests scoreboard):

```
"&aDaily: &f%jpprison_quest_daily-miner_progress%/%jpprison_quest_daily-miner_goal%"
"&6Claimable: &f%jpprison_quest_daily-miner_claimable%"
```

## 14. Ready-to-use examples

Scoreboard (any scoreboard plugin with PAPI):

```
&7Blocks: &f%jpprison_blocks_mined_formatted% (#%jpprison_top_blocks%)
&7Level: &d%jpprison_pickaxe_level% &8(%jpprison_pickaxe_progress_percent%)
%jpprison_pickaxe_progress%
&bTokens: &f%jpprison_tokens%
&aGems: &f%jpprison_gems%
&6Money: &f%jpprison_vault_money%
&eCombo: &f%jpprison_momentum_multiplier%
&bXP Boost: &f%jpprison_xpboost_multiplier%
&bRobots: &f%jpprison_robots_owned% &8(&e%jpprison_robots_ready% ready&8)
```

Welcome chat / profile:

```
"Level %jpprison_pickaxe_level% | %jpprison_total_levels% levels in %jpprison_enchant_count% enchants"
"Prestige %jpprison_pickaxe_prestige% | Theme %jpprison_pickaxe_color_name%"
"Fortune %jpprison_fortune_level%/%jpprison_fortune_max_level% - next: %jpprison_fortune_next_cost_formatted%"
```

Tablist (TAB plugin):

```yaml
header:
  - "&d&lJPPRISON"
  - "&7Tokens: &b%jpprison_tokens% &8| &7Gems: &a%jpprison_gems%"
footer:
  - "&7Blocks #%jpprison_top_blocks% | Combo %jpprison_momentum_multiplier%"
```

Stats hologram (DecentHolograms):

```
&d&lYOUR PICKAXE
&7Level: &f%jpprison_pickaxe_level% &8(%jpprison_pickaxe_progress_percent%)
&7Tokens: &b%jpprison_tokens% &8(#%jpprison_top_tokens%)
&7Combo: &e%jpprison_momentum_multiplier%
```

## 15. Common errors

- I see `"%jpprison_tokens%"` literally: you do not have PlaceholderAPI, or you misspelled the id (check `/economies/*.yml` and `/enchants/*.yml`, everything in lowercase).
- I get an empty `""`: an enchant/economy placeholder that does not exist, or `n` = 0 / 11+ in the tops, or an offline player in a player placeholder (the tops do work without the player online).
- The top does not change instantly: it is normal, it is cached for 30 seconds.
- `%` inside `chance`: the placeholder no longer includes the `%` (it gives `"12.5"`), add it yourself: `"%jpprison_lucky_chance%%"` -> it shows `"12.5%"`.
- IDs with a hyphen (`token-finder`, `frost-nova`...) are written WITH the hyphen: `%jpprison_token-finder_level%` (correct, not with `_`).

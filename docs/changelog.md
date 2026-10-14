# Changelog

All notable changes to JPPrison are documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Added

- **Boost view** (`/boosts`, alias `/multipliers`): players can now read their
  current payout multipliers in chat — pickaxe XP, autosell, momentum, drops,
  key and enchant luck, upgrade cost plus one rewards row per currency, with
  the `(cap)` marker when `boosts.global-cap` clamps a total. The exact same
  numbers are now shown live on the `/stats` profile card (new multiplier
  card, `boosts-slot` in `menus/stats.yml` v2) and on the pickaxe menu head
  (`{boost-xp}`, `{boost-sell}`, `{boost-momentum}`, `{boost-drops}`,
  `{boost-key}`, `{boost-lucky}`, `{boost-cost}` and `{boost-rewards}` in
  `menus/pickaxe.yml` v5 `head-lore`). `/jpprison boost <player>` keeps the
  full per-source breakdown for admins; `BoostInspector` grew stable category
  ids, raw `placeholders(Report)` and a pure `renderTotals` shared by all
  three surfaces. Messages v68 (`boosts.*` templates + the `/boosts` help
  line), `menus/stats.yml` v2 and `menus/pickaxe.yml` v5 migrations (both
  idempotent; untouched bundled layouts are upgraded, admin edits never
  touched).
- **Boost item give shortcuts** (`/shard`, `/rune`, `/remnant` and `/orb`,
  plus their plurals `/shards`, `/runes`, `/remnants`, `/orbs`): direct forms
  of the `/jpprison <kind> give` commands — same permission
  (`jpprison.admin`), feature flags, messages and tab-complete, with the
  arguments typed straight in (`/shard <player> <shard> [count]`). A leading
  `give` is tolerated (`/shard give <player> <shard>`), so the old spelling
  and copy-pasted commands keep working. Usage hints (`shards.usage`,
  `runes.usage`, `orbs.usage`, `remnants.usage` and the `/jpprison` help
  list) now teach the shortcut (messages.yml v69, idempotent migration).
- **Earnings dashboard** (`/earn`, rebuilt from scratch): the board grew
  from a single enchant ranking into a four-view dashboard on one rail —
  named for players and self-explaining (the bottom card is an always-visible
  "how to read this": where you are plus the three steps). **Summary** shows
  your best source first, then one card per economy plus blocks and pickaxe
  XP — every card clickable as a drill-down into that ranking. **Ranking**
  ranks everything that pays — enchantments AND the built-in systems
  (robots, minions, the clone miner, bombs, drills, manual sales) — with %
  bars and a tendency against the previous period of the same length.
  **History** lists every time window side by side with its total, relative
  bar and direction. **Details** breaks one source down into totals, one
  card per currency and one card per window (the back button becomes "Back
  to ranking"), with session activation counts. A read-only session clock
  (time, distinct paying sources, total, hourly rate) sits at the bottom of
  the rail and the whole board still re-renders live; tendencies only render
  once the session actually covers the previous window. The data core
  (`EarningsTracker` v2) derives everything from ONE aggregate query per
  window (`sourceGains` with a backward offset for the previous period),
  adds session activation counters and pure helpers (`rankSources`,
  `gainTotals`, `totalVolume`, `trendPercent`/`trendState`, `hourlyRate`),
  and now records the real income sources that were missing: robot
  collections, clone and minion payouts, bomb detonations, drill finishes
  and the manual sell commands. `menus/earn.yml` v2 exposes the whole layout
  (tabs/session slots, four-row grid, bar glyphs, per-state tendency labels,
  a `sources:` look map and a template per element, incl. a contextual
  `back-detail-item`); the v1→v2 migration widens only the untouched grid,
  and `messages.yml` v67 adds the missing `/earn` help line.
- **Player profiles** (`/stats [player]`, alias `/profile`, `menus/stats.yml`):
  a read-only profile card for any online player — their own head (skinned
  with the session profile, never a Mojang lookup) plus one card per domain,
  all read live from the database: blocks mined + server rank + milestone
  progress (next tier and blocks remaining), pickaxe level/XP/progress
  bar/prestige, every economy balance with one line per currency, enchant
  summary (owned / total levels / maxed), collection (unlocked pets, active
  pet, owned + ready robots, equipped armor + level) and leaderboard
  positions (blocks, XP and one row per economy). Playtime comes from the
  vanilla `PLAY_ONE_TICK` statistic. New `features.stats` master switch
  (`config.yml` v22), inline buttons + filler support like every menu, and
  an admin reference page in `/guide`.
- **Daily & weekly quests** (`/quests`, alias `/quest`, `quests.yml` +
  `menus/quests.yml`): a quest board with automatic goal tracking and manual
  claims. Six goal types fed by the systems' own single choke points (never
  polling): `mine-blocks` (every mined block — manual breaks, enchant
  sweeps, the clone and minions all report material-aware progress through
  `JPPlugin.addBlocksMined`, so `mine-block` can target one ore),
  `earn` (boosted deposits of an economy via a new `EconomyManager` deposit
  observer — admin give/pay never counts), `collect-robot` (collected units
  of one economy, or collection events), `prestige` and `open-crate` (real
  openings, never the admin test show). Two cadences with their own reset
  window (`daily-reset-hour`, `weekly-reset-day`/`weekly-reset-hour`, server
  local time; progress of an expired period resets in place), cross-goal
  completion notice, click-to-claim with progress bars, per-state statuses
  and countdowns, optional per-quest `broadcast`. Claims persist the moment
  they happen (`jpprison_quests`, one row per player + quest: a crash can
  never pay twice) and are wiped by `/jpprison reset`. Bundled catalog: 4
  daily + 3 weekly quests (mining, ores, income, robots, prestige, crates)
  whose rewards are plain console commands. New `features.quests` master
  switch (`config.yml` v22), `quests.*` messages (v66), PAPI
  `quest_<id>_progress|_goal|_claimable|_claimed`, `quests.yml` included in
  backups, admin reference page in `/guide`. The guide hub also self-heals
  on existing installs now: a new merge migration injects any missing
  bundled hub heads (robots/luckyblocks/milestones/stats/quests) into free
  slots without ever touching admin items.
- **Block milestones** (`milestones.yml`, auto-created): one-time rewards the
  moment a player's mined-block counter reaches a configured `blocks:`
  threshold. Every counter source counts — manual breaks, destructive enchant
  sweeps, the autominer clone and the minions all route through the new
  `JPPlugin.addBlocksMined` choke point — and each threshold pays exactly once
  per player: claims are written to `jpprison_milestones` the instant they
  happen (crash-safe, never double-paid). Each entry configures console
  `commands` (`{player}`), an optional private `message:` override, an
  optional server-wide `broadcast:`, a `sound:` override and `enabled:`;
  crossing several thresholds at once pays them in ascending order.
  `retroactive: false` (default) marks already-passed thresholds claimed
  silently on first join so only new crossings pay; `true` pays the backlog
  on the next mined block. Bundled ladder: 1K First Steps → 10K Stone
  Breaker → 50K Deep Digger → 250K Mine Veteran → 1M Block Baron → 5M Prison
  Legend. New `features.milestones` master switch (`config.yml` v21),
  `milestones.*` messages (v65), `milestones.yml` included in backups, admin
  reference added to `/guide`.
- **LuckyBlocks** (`luckyblocks.yml` + `luckyblocks/`, one file per type,
  bundled `common`/`rare`/`legendary`): rare player heads that drop straight
  into the inventory while mining (per-type `chance`, rolled in file order,
  flag-gated like loot chests). They arrive SEALED — a gray head whose lore
  asks for a number of mined blocks, tracked on the item itself (no database:
  trade, store or bank one and its progress travels with it). Every block
  broken with one in the inventory fills it until it unseals: the head
  texture swaps to the real LuckyBlock with a sound, title and chat line.
  Right-clicking an unsealed one consumes 1 and runs its configured console
  reward bundles (`{player}` resolved): plain strings always run, `chance:`
  bundles roll independently, `weight:` bundles enter a one-per-reveal
  weighted raffle (relative odds — a different command can come out each
  time) and `display:` lines print a reveal-chat summary of what was won.
  Per-type `unlock-title: false` mutes the seal-break title (chat + sound
  stay); optional server broadcast. Three bundled
  rarities (gray/TNT common 0.5% at 750 blocks, blue/Lapis rare 0.15% at
  5,000, black/red-eyed legendary 0.04% at 25,000) and a full per-type look
  (textures, names, lore, sounds). Admin give:
  `/jpprison luckyblock give <player> <id> [count] [ready]`. New
  `features.luckyblocks` master switch (`config.yml` v20), `luckyblocks.*`
  messages (v63), `luckyblocks/` included in backups, admin reference added
  to `/guide`.
- **Robots** (`robots.yml` + `robots/`, one file per robot): passive economy
  workers with textured head icons in the `/robots` menu. An admin gives a
  robot (`/jpprison robot give|remove <player> <robot>`, `list` to inspect,
  works offline); it then banks its configured currency at
  `amount-per-second` — online AND offline — into an internal storage capped
  by `max-stored-seconds` (global or per robot). Players click a robot to
  collect its storage or use the COLLECT ALL button; collection deposits
  through the economy layer (timed `/<eco>` boosts apply, the earnings recap
  counts it) and the menu refreshes live every second. Accrual is stored as
  amount + timestamp moved together, so the periodic DB flush is crash-safe
  (no double-paid offline time). **Multi-economy robots**: a robot may pay
  several currencies at once via an `income:` map (`tokens: 10`, `gems: 1`)
  — every payout keeps its own independent rate and storage (same hours of
  production each), the lore renders one row per currency (lines carrying
  payout tokens repeat per economy), and collecting deposits every economy
  (chat summarizes with `robots.collected-multi`). The old single-economy
  files keep working through the `economy`/`amount-per-second` shorthand and
  the pre-multi storage migrates itself into the first payout. **Robots
  level up with use**: collecting grants XP equal to the seconds of
  production gathered, every level raises the robot's incomes and storages
  (capacity always holds `max-stored-seconds` of production) and level ups
  play a sound + title + chat line. The curve, cap and progress bar live in
  a global `robots.yml` `leveling:` section; each robot may cap itself with
  `max-level:` or a full `leveling:` override. New `features.robots` master
  switch (`config.yml` v19), `robots.*` messages (v60, level lines v61,
  multi collect v62), bundled four-robot fleet incl. the multi-economy
  `hybrid-bot`, `robots/` included in backups, admin reference added to
  `/guide`, PAPI
  `robots_owned`, `robots_ready`, `robot_owned|stored|rate|cap|economy|level|xp|xp_needed|max_level|bonus_<id>` plus per-economy
  `robot_stored|rate|cap_<id>_<economy>`.
- **Crates** (`crates/`, one file per crate, auto-created defaults for
  `vote`, `common` and `legendary`): giveable crate items that play a 3D
  opening show when right-clicked. The show is six acts - the crate charges
  on the ground while a particle ring contracts into it and a riser sound
  climbs, it eases up while a light column grows under it, the loot items
  spiral out of it into a ring that winds up, spins fast and eases down
  with a rising tick sound, then the crate dissolves, the losers pop away
  one by one and the weighted winner glides up, is named and is paid out
  with a celebration burst and the title. Each entry has `weight` (relative
  chance), a display item (material/data/head texture/glow), console
  `commands` with `{player}`, an optional private `message`, an optional
  server `broadcast` and a win `sound`; the crate file tunes the whole show
  - shape/timing (`height`, `radius`, `duration-ticks`, `loot-display`,
  `charge-runs`, `reveal-runs`, `spin-start`, `spin-end`), the particle
  theme (`color` + `beam`/`ground-ring`/`sparkles`) and the full sound cue
  sheet (`open-sound`, `charge-sound` riser, `tick-sound` with
  `tick-every`/`tick-pitch-start`/`tick-pitch-end`/`tick-volume`,
  `reveal-sound`, `lose-sound`, `win-sound`). Every animation key is
  optional, so existing crate files get the full new show on reload with
  no migration. Give with `/jpprison crate give <player> <crate> [count]`,
  preview with `/jpprison crate open <crate>`. Interrupted shows
  (quit/reload/shutdown) still pay the rolled reward offline. New
  `features.crates` master switch (`config.yml` v17), `crates.*` messages
  (v57), `crates/` included in backups, admin reference added to `/guide`.
- **Pickaxe level-up notification toggle**: the pickaxe settings menu now has
  a fifth switch (slot 15) that mutes the complete level-up notification —
  chat message, title and sound — per player. Stored in `chat-toggles.yml`
  like the loot/recap toggles; the pickaxe lore still refreshes.
- **Zones** (`zones.yml`, auto-created, reloaded with `/jpprison reload` and
  included in backups): per-WorldGuard-region mining rules matched by region
  id — `disabled-enchants` (enchants that never trigger at that spot),
  `disabled-features` (`autosell`, `enchants`, ...), and
  `reward-multiplier` / `sell-multiplier` / `xp-multiplier` paying on top of
  the `autosell.yml` audit (after `boosts.global-cap`, like timed boosts).
  New `JPPrisonAPI` options: `getZoneIds` / `getZoneIdsAt` /
  `getRegionIdsAt` / `isEnchantDisabledAt` / `isFeatureDisabledAt` /
  `isFeatureEnabledAt` / `getZoneRewardMultiplier` / `getZoneSellMultiplier`
  / `getZoneXpMultiplier` (all null-safe, never throw).

### Changed

- **Economy boost enchants now scale the autosell block value too**: the
  `sell:` payout of `autosell.yml` (normal breaks and every destructive
  sweep) is multiplied by the enchants whose `multiplier-target` matches the
  payout economy (`vault` resolves to `money`), inside the same
  `boosts.global-cap` as fortune/greed/prestige/shards/runes/pets. Previously
  those enchants boosted the per-economy rewards but not the block's sell
  price, which is the biggest part of the payout for most mines. `/sell` and
  `/sellall` are unchanged.
- Autosell now pays only inside the `jpprison-enchants` flag: outside any
  region (or on `deny`) an enabled `/autosell` is ignored and the mined block
  drops into the inventory instead of being sold.
- Economy boosts now use `/<economy> boost <player|global> <multiplier> <time>`
  (e.g. `/tokens boost Steve 2x 30s`); the previous spelling still works, and
  multipliers accept the `2x` form on `/xpboost` too. Each economy keeps its
  own independent boost timers.
- **Crate show rebuilt** (`CrateShow` + `CrateManager`): the choreography is
  now pure unit-tested math (run timeline, easing curves, ring motion,
  ground/beam geometry, tick-sound ramp and the staggered loser pops) and the
  show reads as six distinct acts instead of one continuous spin. The visuals
  go through the modern `Particle` API with a legacy `Effect` fallback, so the
  show no longer collapses into plain smoke on 1.21 where most legacy `Effect`
  constants were removed. The bundled crates get per-crate particle themes and
  distinct cue sheets (aqua/green/gold), and every new animation key is
  optional so existing crate files get the full show on reload with no
  migration.
- **Crate show slowed down and smoothed out** — it read as frantic and
  choppy. The task now ticks once per tick (20 Hz instead of 10) so the
  teleported stands interpolate smoothly, the ring keeps a calm ~1.8s per
  revolution (`spin-start` 30 -> 9 deg/tick) with a **sustained whirl for the
  first 45% of the spin before a smoothstep brake** instead of collapsing
  speed immediately, every act got longer (charge 0.5s, rise 0.7s, orbit 0.7s,
  a 2.5s hold on the winner before and after the payout), the bob amplitudes
  and periods are calm (seconds, not frames), the crate **shudders for the
  last 0.3s** before locking in (the gacha tell), the ring ticks ~7 times per
  second instead of 20 (its pitch now climbs only across the brake window,
  where it doubles in rate as the tension ramp) and the continuous particle FX
  draw at 10 Hz to keep the packet budget sane at the higher tick rate.
- **Crate rings carry more, more representative items**: every bundled loot
  entry now has its own material (9-10 distinct per crate, so each orbiting
  prize is recognisable at a glance) and the fat tiers are splits of one
  another - the expected payout of every economy is unchanged, only the
  variance grew. Ring radii grew to 2.6-3.2 so the new names never overlap.

### Added

- **Crates can pay more than one prize** (`crates/*.yml` `payout-count`,
  1-10, default 1): one open pays N weighted loot entries. The ring still
  reveals a single prize (the gacha suspense is untouched) and the rest arrive
  as bonus prizes at the payout, announced by the new `crates.won-extra` line
  (`{list}` = the extras joined). `allow-duplicate-loot` (default false) keeps
  the prizes distinct: an entry already drawn leaves the pool, and a crate
  asking for more prizes than its pool holds warns at load instead of
  repeating silently. The bundled crates all ship at 1 — a multi-prize crate
  pays several times the expected value, so it is a balance change and is
  never imposed silently.
- **Server-wide crate broadcasts are now a switch, not per-entry busywork**
  (`crates/*.yml` `broadcast:` + `broadcasts.crates` in config.yml): a crate
  can carry ONE line that covers every loot entry without its own, and the
  tri-state `default` | `all` | `none` decides whether authored lines, every
  win (using the new `messages.yml crates.broadcast` default), or nothing at
  all is announced. `default` is the shipped value, so existing installs are
  never surprised with new chat. `messages.yml` v58, `config.yml` v18.
- **Crate ring holograms** (`crates/*.yml` `animation.hologram`, on by
  default): a floating name above **every** orbiting loot item, so the ring
  reads as "these are the prizes" instead of a blur of near-identical blocks.
  Each name rides its item and faces the viewer (a spinning name tag is
  unreadable otherwise) and dies with its item at the reveal, where the winner
  takes over as its own name tag. Configurable with `hologram` (off = plain
  ring), `hologram-label` (`{loot}` = the entry, `{crate}` = the crate) and
  `hologram-height`. Same name-tag-only armor stand look the loot chest
  holograms use, so it renders on 1.8 and is covered by the existing
  damage/interact guards.

### Fixed

- **Crate ring spun at double speed on existing installs**: the show now ticks
  once per tick instead of every 2, and `spin-start`/`spin-end` are degrees per
  tick, so a crate file written before the retune (e.g. `spin-start: 30`) read
  as two revolutions per second and the loot became unreadable.
  `ConfigMigrator.migrateCratesSection` rewrites those files on enable/reload
  (`config-version: 2` per crate file: spin speeds halved, run counts doubled,
  a legacy `tick-every: 1` raised to 3). `duration-ticks` needs no change — the
  old and new spin windows are the same wall-clock length. On top of the
  migration the effective ring speed is hard-capped at
  `CrateShow.MAX_SPEED` (two revolutions per second), so no file, hand-edited or
  not-yet-migrated, can make the orbit frantic.

### Fixed

- Enchant levels in the pickaxe lore render as plain numbers (`1`, `2`, ...)
  instead of Roman numerals (`I`, `II`, ...).
- The crate win sound played twice on every open (once at the reveal and
  again at the payout, one second apart). The reveal now plays the lighter
  `reveal-sound` and the win cue fires exactly once, at the payout.
- **Crate loot looked frozen at the start of the show**: every part spawned
  on the exact same point and then sat on a 0.35-block blob at a single height
  while the ring returned zero speed for the whole charge and rise (0.7s), so
  the client rendered one stacked lump that appeared frozen. The loot now
  spawns already spread and layered on a real pre-orbit ring (45% of the
  orbit radius, widened with the pool size so neighbouring items never merge)
  and the ring turns from the very first frame instead of being parked.
- **`SQLITE_BUSY: The database file is locked` spam** (`getRankBlocks` and the
  other PlaceholderAPI rank queries, called from an async scoreboard while the
  60s cache flush writes): every operation opened its own connection against a
  rollback-journal database, so hundreds of little write transactions in a
  flush burst blocked the readers. `data.db` now runs in **WAL** journal mode
  (readers never block on a writer, set once at init with a single warning if
  the filesystem refuses it) and every connection waits up to 10s for a
  competing writer instead of failing. Because live commits land in
  `data.db-wal` first, `flushAll()` and the backup path checkpoint the WAL
  (`PRAGMA wal_checkpoint(TRUNCATE)`) before anything copies `data.db` alone,
  so backups keep carrying a complete database. Unit-tested
  (`DatabaseManagerWalTest`).
- **Unequipped boost armor re-equipped itself** ("sometimes I take it off and
  it puts itself back on"): `ensureEquipped` runs on **join** and on **every
  `/armor` open**, and with the row deleted by the unequip it looked exactly
  like a player who had never chosen — so the highest unlocked set was marked
  equipped again and the 5-second repair task put the pieces back on the body.
  Unequipping now records an explicit `ARMOR_NONE` marker row (persisted
  immediately), which is distinct from "never chose"; only an admin reset or a
  new unlock overwrites it. Unit-tested
  (`ArmorManagerTest.unequipSticksAcrossMenuReopenAndRejoin`).
- **Every shift-click of a skull was cancelled while wearing boost armor**
  ("the plugin won't let me move any skull in my inventory with shift+click"):
  `ArmorListener` cancelled *every* `MOVE_TO_OTHER_INVENTORY` click of any
  wearable item whose matching armor slot held a set piece — and since the
  base set auto-equips on join, that meant every skull/helmet/pumpkin in the
  player's own inventory (luckyblocks, orbs, pet unlock items, crates...).
  Vanilla quick-move only ever targets an **empty** armor slot, so an
  occupied one makes the quick-equip branch fail and the stack falls back to
  the hotbar/main swap — nothing could be displaced. The blanket cancel is
  gone (same for the equally broad double-click collect cancel: a collect
  only matches the cursor's full item data, and any cursor carrying a marked
  piece was already blocked). All real displacement paths (clicks on the
  worn slot, the piece's own clicks, hotbar swaps, drags, dispensers, armor
  stands) stay guarded, and the repair pass reclaims anything exotic.
  Unit-tested (`ArmorListenerTest`).
- **Shift-clicks in your own inventory did nothing while a menu was open**:
  the router cancelled them so items could never enter the locked menu. They
  are now turned into a hotbar↔main quick-move inside the player inventory
  (merge into a compatible stack first, then the first free slot of the
  other region — vanilla order), so reorganizing items (skulls included)
  works everywhere without ever touching the menu. Pure helper + tests
  (`MenuListenerQuickMoveTest`).

### Fixed

- **Only `common` LuckyBlocks ever worked** (rare/legendary never dropped and
  `/jpprison luckyblock give <player> rare` answered "Unknown luckyblock"):
  the sealed + ready display names ARE the item identity (no NBT on 1.8), and
  the bundled trio shipped the **same** sealed name for every type
  (`&7&lSealed LuckyBlock`), so the loader rejected every file after the
  alphabetically first one ("first file wins") — `legendary` and `rare` were
  silently skipped at startup. The bundled defaults now ship unique sealed
  names (`&7&lSealed Rare LuckyBlock` / `&7&lSealed Legendary LuckyBlock`;
  `common` keeps the generic one so sealed items already in players'
  inventories keep matching and progressing), v1 data files still carrying
  the exact old bundled pair are renamed on enable/reload by
  `ConfigMigrator.migrateLuckyBlockSealedName` (`LUCKYBLOCKS_VERSION` 2,
  idempotent; custom files are never touched), the collision warning now
  names the clashing display name and the key to rename, and `LuckyBlockTest`
  guards cross-file name uniqueness (translated, exactly like the loader) so
  the trap cannot ship again.

## [1.0.0] - 2026-09-18

First public release. One jar, Minecraft 1.8.8 to the latest release, with no
NMS and no hard dependencies (WorldGuard, Vault and PlaceholderAPI are all
optional).

### Added

- **Pickaxe enchants**: data-driven enchant files under `enchants/` (add new
  enchants without code changes), 35 bundled enchants, visual upgrade menu,
  per-enchant unlock levels, per-enchant prestige and activation FX/messages.
- **Custom economies**: one YAML file per economy under `economies/` gives it
  a name, symbol, color, decimals, its own `/<id>` command, leaderboard and
  pay/give/remove support. Vault bridge exposes one economy to shop plugins.
  In-game economy creator menu. Universal `JPPrisonAPI` + `BalanceChangeEvent`.
- **Autosell**: per-block prices and XP in `autosell.yml`, global/per-player
  toggle, works with every enchant, bomb, drill, autominer and minion.
- **Break-type enchants**: explosive, nuke, thunder, meteor, cross, layer,
  solar, frost nova, geyser, soul spiral, comet, volcano, blackhole, aurora,
  sandstorm and apocalypse, each with progressive animations, region-aware
  sweeps and full reward pipeline.
- **Bombs and drills**: throwable TNT skulls and placeable hoppers, fully
  data-driven types, flag-gated blasts/shafts, bulk reward payouts and a
  refund when a blast fizzles.
- **Autominer**: a visible clone wearing your skin that walks the mine and
  mines for you, time-based, with 10 permanent upgrade modules.
- **Minions**: miniature baby mobs (zombie, pigman, villager) mining on their
  own, feedable with time items, each with its own upgrades.
- **Pets**: 13 floating-head pets with offline-safe textures, pet levels,
  per-economy boosts, mini armor, animations, particle trails, unlock items
  and a lucky-block unlock show.
- **Shards and runes**: socketable boosts with 13 effect types, progressive
  slot unlocks and safe insert/extract.
- **Boost armors**: 10-set unlock chain with real wearable pieces, per-level
  growth, live progress bars and anti-theft protection.
- **Loot chests**: holograms, custom animations/sounds, claim messages and
  five bonus minigames (needle, sequence, smash, ladder, lucky pick).
- **Prestige**: global pickaxe prestige (tiers, costs, multipliers) and
  per-enchant prestige (cap, chance, multiplier, radius, rewards, XP and
  cost bonuses).
- **Momentum combos**: continuous-mining multiplier with milestone titles,
  sounds and rewards.
- **Timed boosts**: `/xpboost` and `/<economy> <player|global> <mult> <time>`
  for events, plus a boost inspector (`/jpprison boost`) and a global stacking
  cap.
- **Leaderboards**: `/blockstop`, `/xptop`, `/<economy> top` with offline
  player support.
- **Earnings recap and earn board**: automatic 1m/5m chat summaries and a
  live `/earn` board ranking which enchants pay the most.
- **XP orbs and enchant remnants**: giveable items for crates, kits, vote
  rewards and stores.
- **WorldGuard integration**: custom `jpprison-enchants` flag (WorldGuard 6
  and 7) gating all mining rewards, plus protection from grief outside mines.
- **Quality of life**: automatic versioned config migrations, automatic
  backups, feature flags for every module, universal placeholders, full menu
  system driven by YAML, cross-version safety (1.8 through latest), and
  optional metrics/update-checker switches (off by default).

### Notes

- Storage is SQLite by default (zero configuration).
- No NMS, no Adventure and no `PersistentDataContainer`; the plugin runs
  unchanged from 1.8.8 to current servers.
- Requires Java 8 or newer on the server.

# Features

Everything you need to run a modern Prison server, in one plugin. This page is the full tour — each system is data-driven and reloadable with `/jpprison reload`.

## 1. Pickaxe enchants

**35 unique enchantments for your pickaxe, upgraded in a visual menu with your own in-game currencies.**

- Explosive, laser-like and area-breaking enchants with effects
- Upgrade levels, unlock requirements and per-enchant prestige
- Multipliers that boost everything you earn while mining
- New enchants can be added without touching code

## 2. Custom economies

**Not just money: run Tokens, Gems and any currency you invent — each with its own command, balance and leaderboard.**

- Every currency has its own `/command`, symbol, color, decimals and top list
- Vault support, so other shop plugins can charge your main currency
- Secure player-to-player payments, with confirmations
- Add a brand-new economy just by adding a file

## 3. Autosell

**Blocks turn into money the instant you mine them. No chests full of cobblestone, no trips to the shop.**

- Per-block prices and XP, fully configurable
- Toggle it on or off whenever you want
- Works with every enchant, bomb, drill, miner and minion

## 4. Bombs & drills

**Throwable TNT skulls that carve paying craters wherever you aim, and drills that bore vertical shafts on their own while you do something else.**

- Several bomb types: balanced, nukes, fast minis and more
- Drills in multiple sizes, speeds and reward multipliers
- Respect protected regions — never grief outside the mines
- One summary of everything every blast and shaft earned you

## 5. Autominer — your clone miner

**Your own clone that walks the mine, breaks blocks and pays you as if you mined every block yourself.**

- Visible clone wearing your skin and your name
- Walks with a real path, never through walls
- Time-based: admins give minutes, the clone spends them mining
- One-button start/stop menu with live stats

## 6. Minions — your mob crew

**Real miniature mobs (baby zombie, baby pigman, baby villager) that mine for you. Deploy up to 3 at the same time.**

- Each mob has its own personality: fast, balanced or heavy packs
- Feed them time items to keep them working
- Right-click any mob for its own options and upgrades
- New mob types can be added without touching code

## 7. Robots — passive economy workers

**Give your players textured robot workers that generate income non-stop: each robot banks its currency around the clock — even while its owner is offline — until it is collected.**

- One YML file per robot: currency, income per second, skull skin
- One robot can pay several currencies at once (`income:` map), each with its own independent rate and storage
- Internal storage with a cap; collect anytime from the `/robots` menu
- Online **and** offline production, with crash-safe accounting
- Robots level up while you collect: every level raises income and storage
- New robots can be added without touching code

## 8. Pets

**Floating heads that follow you everywhere and boost everything you do: sell prices, XP, rewards and combos.**

- 13 themed pets with offline-safe custom heads
- Pets level up by mining and grow stronger with every level
- Unlock items, lucky blocks and a full customization menu
- Mini armor, animations and particle trails, per pet

## 9. Shards & runes

**Socket magical shards and runes into your pickaxe for passive boosts: XP, rewards, luck, speed, cheaper upgrades and more.**

- 13 boost types, mix and match freely
- Slots unlock as your pickaxe levels up
- Insert and extract with simple clicks — nothing is ever lost

## 10. Boost armors

**Ten armor sets forming an unlock chain, each stronger than the last, with real wearable pieces and live progress bars.**

- Equip sets that boost mining, rewards, XP and movement
- Level every set just by playing
- Never lose power: each set starts where the last one peaked

## 11. Loot chests

**Treasure chests that pop out of the mine while you dig, with holograms, effects and bonus skill minigames.**

- Needle, memory, smash, ladder and lucky-pick bonus games
- Custom animations, sounds and claim messages per chest
- Claim streaks with combo rewards

## 12. Crates

**Giveable crate items that open with a 3D roulette show: the crate item floats in the middle while all possible loot orbits it, then the ring slows down and reveals your weighted prize.**

- Configure the weight (chance) of every loot entry
- Prizes run commands, broadcast to the server, play sounds
- One YAML file per crate; item, animation and loot all editable

## 13. Prestige system

**Reset your pickaxe at max level for permanent account-wide bonuses — and prestige single enchants even further.**

- Global prestige: +rewards and +XP forever, up to 100 levels
- Per-enchant prestige: bigger radius, better chance, new caps
- Fully announced with titles, sounds and effects

## 14. Momentum combos

**Keep mining without stopping to build a combo multiplier that boosts everything, with milestone celebrations along the way.**

- Combo grows the longer you mine nonstop
- Milestone titles, sounds and bonus commands
- Visible live on scoreboards

## 15. Timed server boosts

**Launch double-XP weekends (or any multiplier, any currency, any duration) with one command — for one player or the whole server.**

- XP boosts and per-currency boosts, stackable with everything
- Flexible times: `30s`, `10m`, `2:30m`, `2h`, `1d`...
- Players check their own multipliers anytime with `/boosts`
- Perfect for events, vote rewards and releases

## 16. Leaderboards

**Top-10 rankings for blocks mined, pickaxe XP and every currency, including offline players.**

- `/blockstop`, `/xptop` and `/<currency> top`
- Scoreboard-ready placeholders for every rank and value

## 17. Earnings recap & dashboard

**Automatic chat summaries of what you earned every minute and every 5 minutes, plus a full earnings dashboard: where every coin comes from, how much, and whether it is growing.**

- `/earn` summary: your best source first + one card per currency, blocks and XP
- Source ranking for enchants **and** robots, minions, clones, bombs, drills and manual sales, with % bars
- Tendency arrows vs the previous period and a per-window history
- Click any source for its details: per-currency and per-period breakdown
- Session clock with time played, paying sources and hourly rate
- A built-in "how to read this" card, so it explains itself
- Never wonder where your money came from again

## 18. XP orbs & enchant remnants

**Giveable items: orbs packed with pickaxe XP, and remnants packed with enchant levels. Click them onto a pickaxe to apply.**

- Any XP amount, any enchant, any level count
- Great for crates, kits, vote rewards and the store

## 19. Mining regions & protection

**Rewards only happen inside your mining regions, set with a single flag. Outside them, everything stays safe and normal.**

- Works with WorldGuard 6 and 7, or with no region plugin at all
- Bombs, drills, miners and minions all respect the same flag
- Autosell only sells inside the mine; outside, blocks drop into your inventory

## 20. Runs everywhere, worry-free

**One jar from Minecraft 1.8 to the latest version, with automatic backups, crash recovery and safe reloads.**

- Cross-version support with zero setup
- Automatic database backups with rotation
- Crash-proof: no frozen pets, clones, mobs or chests after restarts
- Configs auto-update on every version without losing your edits

## 21. LuckyBlocks — mining prizes

**Rare lucky blocks that drop straight into your inventory while mining. They arrive sealed and gray, with a "mine N blocks" requirement written on them; keep mining with one in your bag and it cracks open into the real lucky block — then right-click to reveal its prize.**

- Sealed gray head unlocks into a colorful lucky block with use
- Progress is stored on the item itself: trade or store it freely
- Weighted raffles: several command prizes, each with its own odds
- The reveal prints a chat summary of exactly what you won
- Title/subtitle on unlock can be toggled off per lucky block
- Three rarities bundled (common, rare, legendary) + endless custom ones

## 22. Block milestones

**One-time rewards the moment a player's mined-block counter hits a configured threshold (1,000 / 10,000 / 50,000...). Every source counts — your pickaxe, enchant sweeps, clone miners and minion crews — and each threshold pays exactly once per player.**

- Configure any ladder of thresholds in `milestones.yml`
- Each tier runs console commands, plays a sound and can broadcast
- Crossing several tiers at once pays them in order
- Retroactive switch: pay the backlog or only future crossings

## 23. Player profiles — `/stats`

**A profile card for any online player: their own head plus one card per domain, all read live from the database.**

- Blocks mined + server rank + milestone progress (next tier and how many blocks are left)
- Pickaxe level, XP, progress bar and prestige
- Every currency balance, one row each
- Enchant summary (owned, total levels, maxed), pets, robots, armor
- Leaderboard positions: blocks, XP and one row per currency
- Current payout multipliers (XP, autosell, momentum, drops, key and enchant luck, upgrade cost + one rewards row per currency) — same numbers as `/boosts` and the pickaxe menu head
- Playtime from the vanilla statistic; `/stats [player]` to view others

## 24. Daily & weekly quests — `/quests`

**A quest board with automatic goals: mine blocks (or one specific ore), earn a currency, collect from your robots, prestige or open crates — claim the reward with a click before the reset.**

- Two cadences: daily and weekly, each with its own reset hour/day
- Six goal types fed by the systems' own choke points (no polling)
- Manual claims with progress bars, countdowns and reward summaries
- Weighted toward what your server actually runs: rewards are just console commands, so anything can be a quest prize
- One file (`quests.yml`), everything editable, `/jpprison reload`

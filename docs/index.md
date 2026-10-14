<div class="jpp-hero" markdown>
<img class="jpp-hero__crest" src="assets/jpprison-logo.jpg" alt="JPPrison logo">

<p class="jpp-kicker">Minecraft prison core · Spigot &amp; Paper</p>

# JPPrison

<p class="jpp-tagline">Mine. Upgrade. Prestige. Repeat.</p>

<p class="jpp-chips">
<span class="jpp-chip">1.8.8 → latest</span>
<span class="jpp-chip">Java 8+</span>
<span class="jpp-chip">No NMS · no required dependencies</span>
<span class="jpp-chip jpp-chip--hot">Free with attribution</span>
</p>

<p class="jpp-cta" markdown>
[Quick start :material-rocket-launch:](getting-started/quick-start.md){ .md-button .md-button--primary }
[Full feature tour :material-map-legend:](features.md){ .md-button }
</p>
</div>

<div class="jpp-stats">
<div class="jpp-stat"><b>35</b><span>enchants bundled</span></div>
<div class="jpp-stat"><b>24</b><span>systems in one jar</span></div>
<div class="jpp-stat"><b>1.8.8+</b><span>every MC version</span></div>
<div class="jpp-stat"><b>0</b><span>required plugins</span></div>
</div>

JPPrison is an all-in-one Prison core: enchanted pickaxes, multiple economies, autosell, pets, minions, robots, bombs, drills, loot chests, crates, boost armors, prestige, combos and more — one jar that runs from **Minecraft 1.8.8 to the latest release**, with no NMS and no required dependencies.

<div class="jpp-term">
<div class="jpp-term__bar"><i></i><i></i><i></i><b>console — JPPrison</b></div>
<pre><code><span class="t-dim">[12:00:01]</span> Enabling <span class="t-hot">JPPrison v1.0.0</span>
<span class="t-dim">[12:00:01]</span> <span class="t-ok">+</span> 35 enchants loaded from /enchants
<span class="t-dim">[12:00:01]</span> <span class="t-ok">+</span> economies: tokens, gems, money
<span class="t-dim">[12:00:02]</span> <span class="t-ok">+</span> region flag jpprison-enchants registered
<span class="t-dim">[12:00:02]</span> <span class="t-ok">+</span> 13 pets, 3 minions, 4 robots, 3 crates
<span class="t-dim">[12:00:02]</span> Done in 0.4s — have fun mining.
<span class="t-cmd">&gt;</span> /jpprison reload
<span class="t-dim">[12:00:20]</span> <span class="t-ok">+</span> Reloaded 9 config files, 0 restarts needed</code></pre>
</div>

## One plugin, twenty-four systems

<div class="grid cards" markdown>

-   :material-pickaxe: **Pickaxe enchants**

    ---

    35 bundled enchants with visual upgrade menus, activation effects, unlock levels and per-enchant prestige.

-   :material-currency-usd: **Custom economies**

    ---

    Tokens, gems or a currency you invent — each gets its own command, symbol, color and leaderboard.

-   :material-sale: **Autosell**

    ---

    Blocks sell the instant they break. One price table pays your pickaxe, bombs, drills, minions and more.

-   :material-bomb: **Bombs & drills**

    ---

    Throwable blast skulls and placeable shaft borers — data-driven types, flag-safe, one payout summary each.

-   :material-account-hard-hat: **Clone miner**

    ---

    The autominer: a visible copy of you that walks the mine, never clips walls and upgrades through 10 modules.

-   :material-account-group: **Minions**

    ---

    A crew of mini mobs that mine on their own. Feed them time items; every mob upgrades separately.

-   :material-robot: **Robots**

    ---

    Passive workers banking currency around the clock — online or offline — and leveling up as you collect.

-   :material-paw: **Pets**

    ---

    13 floating-head pets with levels, per-currency boosts, mini armor, animations and particle trails.

-   :material-treasure-chest: **Loot chests & crates**

    ---

    Animated rewards with holograms, custom effects, five skill minigames and a 3D crate roulette show.

-   :material-diamond-stone: **Armors, shards & runes**

    ---

    A 10-set wearable unlock chain and socketable boosts covering 13 effect types, unlocked as you level.

-   :material-chart-timeline-variant: **Profile, quests & earnings**

    ---

    `/stats` profile cards, a daily/weekly quest board and the `/earn` dashboard with 1m/5m chat recaps.

-   :material-shield-check: **Regions & protection**

    ---

    One WorldGuard flag gates every reward. No WorldGuard installed? Everything just works everywhere.

</div>

<p class="jpp-actions" markdown>
[Explore all 24 systems :octicons-arrow-right-24:](features.md){ .md-button }
</p>

<div class="jpp-split" markdown>
<div class="jpp-split__text" markdown>

## Configure in YAML, apply with a reload

- Enchants, economies, menus, pets, crates, robots, bombs, drills and loot are single YAML files.
- Edit a file, run `/jpprison reload` — live. No restart, no recompile.
- Every config is versioned and migrates itself on update, keeping your edits and comments.

</div>
<div class="jpp-split__code" markdown>

```yaml title="enchants/meteor.yml"
id: meteor
name: Meteor
icon: FIREBALL
max-level: 25
cost-per-level: 300
upgrade-economy: gems
break-type: meteor
required-pickaxe-level: 20
radius: 2
radius-y: 2
```

</div>
</div>

## Runs everywhere

| | |
|---|---|
| **Server** | Spigot / Paper **1.8.8 through the latest release** — one jar covers every version. |
| **Java** | 8 or newer. |
| **Storage** | SQLite (bundled, zero configuration). |
| **Dependencies** | None required. |

Optional integrations: **WorldGuard** 6.x/7.x+ (the `jpprison-enchants` region flag gates all mining rewards), **Vault** (expose one economy to shop plugins) and **PlaceholderAPI** (the `%jpprison_<variable>%` expansion).

## From zero to mining in minutes

<div class="jpp-steps">
<div class="jpp-step"><span class="jpp-step__num">01</span><b>Install the jar</b><p>Drop it into <code>plugins/</code> and start — every config file generates itself on first run.</p></div>
<div class="jpp-step"><span class="jpp-step__num">02</span><b>Flag your mine</b><p>Turn a mine into a reward zone with one WorldGuard command: <code>/rg flag &lt;region&gt; jpprison-enchants allow</code>.</p></div>
<div class="jpp-step"><span class="jpp-step__num">03</span><b>Price your blocks</b><p>Set the sell value, XP and per-currency rewards of every block in <code>autosell.yml</code>.</p></div>
<div class="jpp-step"><span class="jpp-step__num">04</span><b>Hand out test kits</b><p>Orbs, shards, bombs, drills, crates — grab one of each and mine for a minute.</p></div>
</div>

<p class="jpp-actions" markdown>
[Read the quick start :octicons-arrow-right-24:](getting-started/quick-start.md){ .md-button }
</p>

## Going deeper

<div class="grid cards" markdown>

-   :material-map-legend: **Configuration map**

    ---

    What lives in which file, the 28 feature flags and how migrations keep your edits.

    [:octicons-arrow-right-24: Configuration](getting-started/configuration.md)

-   :material-console: **Commands & placeholders**

    ---

    Every command, alias and permission — plus the full `%jpprison_…%` expansion.

    [:octicons-arrow-right-24: Commands](reference/commands.md) · [:octicons-arrow-right-24: Placeholders](reference/placeholders.md)

-   :material-code-braces: **Developer API**

    ---

    Balance API, events and the WorldGuard flag, for your own plugins and shops.

    [:octicons-arrow-right-24: API reference](developer/api.md)

</div>

## Support & license

JPPrison is developed by **MrDriz_** and licensed **free with attribution, no resale**: you may use, modify and redistribute it (including on public and for-profit servers) as long as you keep the credit, and you may not sell the plugin or modified versions. Bundled third-party components keep their own licenses: XSeries (MIT), sqlite-jdbc (Apache-2.0) and bStats (MIT).

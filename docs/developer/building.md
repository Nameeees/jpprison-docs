# Building & Development

This page is for working on the plugin itself (or forking it). To just run it, see [Installation](../getting-started/installation.md).

## Requirements

- **JDK 8 or newer** — the plugin targets Java 8 bytecode.
- **Maven**.

## Build & test

```bash
mvn clean package    # -> target/JPPrison-<version>.jar
mvn test             # JUnit 5 + Mockito test suite
```

`<version>` in `pom.xml` is the single source of truth — `plugin.yml` picks it up through Maven resource filtering (`${project.version}`).

## Running on a dev server

- Baseline: **Paper 1.8.8**. If it works there, it works everywhere.
- Then smoke-test on the **newest Paper** for modern API behavior.
- Copy the jar with the server **stopped** — never hot-swap a running jar. The JVM keeps the old file handle and lazily-loaded classes fail with `ClassNotFoundException` after the swap.

## Project layout

```text
src/main/java/com/jpprison/
├── JPPlugin.java            plugin core: managers, commands, listener wiring
├── economy/                 economies, JPPrisonAPI, BalanceChangeEvent, EconomyCost
├── enchant/                 enchant definitions, procs, break handler, enchant prestige
├── menu/                    unified menu runtime (controllers, sessions, renderer)
├── pet/  minion/  robot/    companion systems
├── autominer/  bomb/  drill/  autonomous mining tools
├── loot/  crate/  luckyblock/  milestone/  quest/   reward systems
├── boost/                   shared insertable-boost framework (shards, runes, armors)
├── zone/                    per-region mining rules
├── update/  metrics/  backup/   public-release extras
└── util/                    cross-version utilities (materials, sounds, effects, skulls)
```

## Conventions that keep the plugin cross-version (1.8 → latest)

- Compile against the **Spigot 1.8.8 API**. No NMS, no Adventure, no ProtocolLib, no `PersistentDataContainer`.
- Cross-version materials, sounds and particles go through **XSeries** (shaded, relocated to `com.jpprison.libs.xseries`) plus reflection for modern-only APIs, always with graceful fallbacks.
- Chat is `ChatColor` + `§` — a hard rule.
- **Data-driven by design**: a new enchant is a file in `/enchants/`, a new economy a file in `/economies/`, a new pet/minion/robot/lucky block/crate a file of its own. New content never requires code changes.

## Unit tests

- Pure logic (parsers, math, migrations, renderers) lives in `public static` methods with unit tests — `mvn test` must be green before packaging.
- Tests that stub a `JPPlugin` mock must also enable feature gates: `when(plugin.isFeatureEnabled(anyString())).thenReturn(true)`, or gated paths read as disabled.

## Tools in the repo

- `tools/enchant-creator.html` — the offline enchant editor (see [Enchant Creator](../tools/enchant-creator.md)). A build-time test (`EnchantCreatorSchemaTest`) fails the build if the tool's field schema drifts from the plugin.
- `tools/balance-bot` — a Node dev harness that boots a sandbox server copy, measures real per-block payouts with Mineflayer bots, validates config vs game and auto-tunes economy levers into target bands, producing convergence reports per round.

## Versioning & release

- `pom.xml` `<version>` is the single source; `plugin.yml` is resource-filtered, the rest of `resources/` ships unfiltered.
- `README.md`, `CHANGELOG.md` (Keep a Changelog) and `LICENSE` live at the repo root and stay in sync with user-facing additions.
- Public-release switches (`update-checker`, `bstats`) ship **disabled** by default.

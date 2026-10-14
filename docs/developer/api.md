# Developer API

JPPrison ships its integration surface inside the plugin jar — compile against `JPPrison.jar` as a `provided` dependency. All entry points below are **static, null-safe and never throw**: unknown ids, null players and missing plugins degrade gracefully instead of breaking your code.

## Balance API — `com.jpprison.economy.JPPrisonAPI`

```java
import com.jpprison.economy.JPPrisonAPI;
import com.jpprison.economy.EconomyManager;

// Reads (every economy id from /economies/*; "money" when the Vault bridge is up)
double balance = JPPrisonAPI.getBalance("tokens", player.getUniqueId());
boolean canPay = JPPrisonAPI.has("gems", player, 500);      // UUID or OfflinePlayer overloads

// Writes — unboosted by design (timed boosts only apply to mining income).
// Every write fires BalanceChangeEvent first; the result tells you the outcome.
JPPrisonAPI.give("tokens", uuid, 1000, "quest_reward");
JPPrisonAPI.take("gems", uuid, 25, "shop");

// Atomic purchase: checks funds and withdraws under a single lock.
// Never do has() + take() yourself — races and double-spends live there.
if (JPPrisonAPI.tryPurchase(uuid, "tokens", 500, "shop")
        == EconomyManager.TransferResult.OK) {
    // deliver the goods
}

// Formatting & discovery
String text = JPPrisonAPI.format("tokens", 1250);          // "1,250 Tokens"
boolean exists = JPPrisonAPI.exists("keys");
for (String id : JPPrisonAPI.getEconomyIds()) { ... }
```

### Offline-safe player resolution

```java
OfflinePlayer target = JPPrisonAPI.resolvePlayer("Steve");
// exact online name -> online prefix -> offline player with hasPlayedBefore
// null when nobody matches
```

### `TransferResult`

`give`, `take` and `tryPurchase` return `EconomyManager.TransferResult`:

| Constant | Meaning |
|---|---|
| `OK` | Completed. |
| `UNKNOWN_ECONOMY` | No such economy id (or null input). |
| `INVALID_AMOUNT` | NaN, infinite or ≤ 0. |
| `INSUFFICIENT` | Not enough balance (`tryPurchase`). |
| `EVENT_CANCELLED` | A `BalanceChangeEvent` listener vetoed the write. |
| `BACKEND_ERROR` | Storage / Vault failure. |

## Prices in any economy — `EconomyCost`

A generic price tag (economy id + amount) that any feature can use — menus, chests, drills, external shop plugins:

```java
import com.jpprison.economy.EconomyCost;

EconomyCost cost = EconomyCost.parse("500 tokens");   // "tokens 500" and "tokens:500" also work
EconomyCost fromYml = EconomyCost.parse(config.getConfigurationSection("cost"));
// YML: { economy: tokens, amount: 500 }
// aliases: id / currency / eco        price / cost / value

if (JPPrisonAPI.tryPurchase(uuid, cost, "shop") == EconomyManager.TransferResult.OK) {
    // deliver
}
```

- `parse(String)`, `parse(String, String defaultEconomy)`, `parse(ConfigurationSection)` and `parse(Map)` are pure and null-safe — invalid input returns `null`, never throws.
- Amounts are validated (finite, not NaN, `> 0`); `withdraw` floors at 0 and `deposit` clamps to the economy's `max-balance`.

## Events — `BalanceChangeEvent`

Cancellable and synchronous, fired **before every balance write** (both file economies and the Vault-backed one). No veto → the write applies.

```java
import com.jpprison.economy.BalanceChangeEvent;
import org.bukkit.event.EventHandler;
import org.bukkit.event.Listener;

@EventHandler
public void onBalance(BalanceChangeEvent event) {
    // getPlayerId() · getEconomy() · getEconomyId() · getOldBalance()
    // getNewBalance() · setNewBalance(double) · getDelta() · getCause()
    if ("quest_reward".equals(event.getCause()) && event.getDelta() > 10_000) {
        event.setCancelled(true);          // veto: abort the write
        // - or -
        event.setNewBalance(10_000);       // adjust it
    }
}
```

- `getCause()` is a transaction id: built-ins `reward`, `autosell`, `admin_give`, `admin_remove`, `pay`, `purchase`, `plugin`, `vault`, `unknown` — free-form ids from integrations are kept as-is.
- Fired only on the primary thread; a write from another thread still applies, just without the event.

## WorldGuard integration

```java
Object flag = JPPrisonAPI.getWorldGuardFlag();      // canonical flag instance or null
String name = JPPrisonAPI.getWorldGuardFlagName();  // "jpprison-enchants"
```

- With WorldGuard installed, mining rewards require the `jpprison-enchants` flag to be **defined** at the block: `ALLOW` pays, `DENY` blocks rewards, undefined pays nothing (and autosell is skipped).
- The flag *instance* matters: WorldGuard keys regions by flag identity. Always fetch it through the API — a `new StateFlag("jpprison-enchants")` would always miss.
- Live helpers with no reflection on your side: `JPPlugin.getWorldGuard()` exposes `hasFlagAt(location)`, `isDeniedAt(location)` and `queryFlagAt(location)`. The flag name constant is `WorldGuardHook.FLAG_NAME`.

## Zones — per-region rules (`zones.yml`)

```java
for (String zone : JPPrisonAPI.getZoneIdsAt(location)) { ... }

boolean muted   = JPPrisonAPI.isEnchantDisabledAt(location, "meteor");
boolean offHere = JPPrisonAPI.isFeatureDisabledAt(location, "autosell");
boolean sellOn  = JPPrisonAPI.isFeatureEnabledAt(location, "autosell");
double reward   = JPPrisonAPI.getZoneRewardMultiplier(location);
double sell     = JPPrisonAPI.getZoneSellMultiplier(location);
double xp       = JPPrisonAPI.getZoneXpMultiplier(location);
```

- Zones disable specific enchants/features and add reward/sell/XP multipliers per WorldGuard region.
- All zone calls are null-safe and **fail-open**: no zones file, no WorldGuard or no match means "no rule" — nothing disabled, ×1.0.
- Also available: `getRegionIdsAt(location)` (the region ids covering a spot) and `getZoneIds()` (every defined zone).

## Vault bridge

- `economy.vault-currency` in `config.yml` (default `money`) selects the ONE economy exposed through Vault; `none` disables the bridge.
- Vault credits from external shops are **unboosted** by design — timed boosts only apply to mining income.

## Where to look in the source

| Package | Contents |
|---|---|
| `com.jpprison.economy` | `JPPrisonAPI`, `EconomyManager`, `EconomyCost`, `BalanceChangeEvent`, `VaultEconomy`. |
| `com.jpprison.enchant` | `EnchantManager`, `EnchantData`, `BreakEnchantHandler`, `EnchantFx`, enchant prestige. |
| `com.jpprison.menu` | `MenuManager`, `MenuController`, `MenuSession`, `MenuRenderer`, `MenuItemData`. |
| `com.jpprison.zone` | `ZoneManager`, `ZoneData` — per-region rules. |
| `com.jpprison.config` | `ConfigMigrator` — versioned migrations. |
| `com.jpprison.storage` | SQLite layer (cache flushes, WAL, backups). |
| `com.jpprison.util` | Cross-version helpers: `MaterialUtil`, `SoundUtil`, `EffectUtil`, `SkullUtil`, `TitleUtil`, `ColorUtil`... |
| `com.jpprison.listener` | The single event pipeline hub where managers hook in. |

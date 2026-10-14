# JPPrison — Balance Analysis

Analysis of the current economy curves based on `autosell.yml`, `enchants/*.yml` and `config.yml`.
Estimates assume **60 blocks/min** (config `mining.blocks-per-minute`) in a mine mixing common blocks
and ores.

## Income per block (from autosell.yml)

| Block | Tokens (avg) | Gems | Sell (money) | XP |
|-------|-------------|------|--------------|----|
| STONE | 50–150 (100) | 8% × 10–30 (~1.6) | 1.0 | 1 |
| COBBLESTONE / SAND / GRAVEL | 30–80 (55) | — | 1.0 | 1 |
| NETHERRACK | 20–60 (40) | — | 1.0 | 1 |
| COAL_ORE | 40–120 (80) | — | 4.0 | 5 |
| IRON_ORE | 50–150 (100) | — | 12.0 | 10 |
| GOLD_ORE | 60–200 (130) | — | 28.0 | 15 |
| LAPIS_ORE | 50–150 (100) | — | 15.0 | 10 |
| REDSTONE_ORE | 40–120 (80) | — | 8.0 | 8 |
| DIAMOND_ORE | 100–300 (200) | 15–40 (27.5) | 75.0 | 25 |
| EMERALD_ORE | 80–250 (165) | 10–30 (20) | 50.0 | 20 |
| NETHER_QUARTZ_ORE | 30–80 (55) | — | 5.0 | 5 |

**Global averages** (all blocks, equal weight):
- **Tokens: ~93.5/block → ~5,600/min → ~337k/hour**
- **Gems: ~16.4/block** (stone 8% chance dilutes a lot) **→ ~984/min → ~59k/hour**
- **Money (autosell): ~17/block → ~1,000/min → ~61k/hour**
- **XP: ~9/block → ~540/min**

## Upgrade costs (sum of `costPerLevel × level`)

| Enchant | cpl | 1→10 | 50→60 | Total to max |
|---------|-----|------|-------|--------------|
| efficiency | 500 | 27,000 | 52,750 | 232,500 (Lv30) |
| fortune | 1,500 | 81,000 | 158,250 | 487,500 (Lv25) |
| token-finder | 5,000 | 270,000 | 527,500 | 6,375,000 (Lv50) |
| explosive | 5,000 | 270,000 | 527,500 | 2,325,000 (Lv30) |
| jackhammer | 6,000 | 324,000 | 633,000 | 2,790,000 (Lv30) |
| meteor | 15,000 | 810,000 | 1,582,500 | 3,135,000 (Lv20) |
| nuke | 20,000 | 1,080,000 | 2,110,000 | 4,180,000 (Lv20) |
| lucky | 15,000 | 810,000 | 1,582,500 | 6,975,000 (Lv30) |

## Time to afford (no multipliers, 60 blocks/min)

| Goal | Cost | Tokens | Gems |
|------|------|--------|------|
| efficiency 1→10 | 27,000 | ~5 min | — |
| efficiency →30 (max) | 232,500 | ~40 min | — |
| explosive 1→10 | 270,000 | ~50 min | ~4.6 h |
| meteor 1→10 | 810,000 | — | ~13.7 h |
| meteor →20 (max) | 3,135,000 | — | ~53 h |
| nuke →20 (max) | 4,180,000 | — | ~71 h |

## Findings

1. **Tokens flow well for starter enchants** (efficiency/fortune < 1 h to max), but the
   economy sinks are the expensive custom enchants: token-finder max costs 6.4M tokens (~19 h).
   Acceptable for a long-term server, but watch inflation: tokens have no other sink.
2. **Gems are the bottleneck (by design), but the curve is too steep**: meteor/nuke to max is
   50–70 h of pure mining. With `gem-finder` Lv50 (×4) it drops to ~13–17 h — meaning
   gem-finder becomes mandatory, and without it players quit the destructive path.
   **Recommendation**: lower meteor/nuke/layer `cost-per-level` to 8,000–12,000, or add a gems
   reward to GOLD/LAPIS/REDSTONE ores (they currently give none), or both.
3. **Money has no sink** (no rankup/shop yet). Autosell + greed-money accumulate indefinitely.
   **Recommendation**: add rankup or a shop before money economy inflates.
4. **Momentum is the strongest multiplier** (+0.8 tokens / +0.3 gems / +1.0 money at 10m) and it
   stacks with finders and lucky. With all multipliers a player can reach ~3–4× base income,
   which roughly halves all the times above. Keep an eye on it once ranks exist.
5. **XP curve**: level N needs N²·100 XP (~9 XP/block base). Level 50 ≈ 250k XP ≈ 125 h
   (≈ 50 h with xp-finder Lv50). Pickaxe level is currently cosmetic (no perks) — when perks
   are added, re-check this curve.

## Suggested first pass (low effort, high impact)

- `meteor.yml` / `nuke.yml`: `cost-per-level` 15000/20000 → **10000**
- `layer.yml`: 10000 → 8000
- Add gems (chance 5%, 5–15) to GOLD_ORE, LAPIS_ORE, REDSTONE_ORE
- Add a money sink (rankup) before launch

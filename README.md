# Infinite Tower Ceiling Calculator

**→ [Open the calculator](https://mjjvanderveldt.github.io/IdleFantasyTowerCalculator/)**

Works out how high you can climb the Infinite Tower in
[Idle Fantasy](https://github.com/mjjvanderveldt/IdleFantasyPremium), from your gear,
your combat levels and the food you bring.

It does not estimate. The page carries a tick-for-tick port of the game's own
`CombatSimulator.simulateDungeon` and runs a few hundred simulated 60-minute floors in
your browser, then reports the clear rate per attempt at each floor.

## What it models

Ported faithfully from `simulator/CombatSimulator.kt` and `simulator/TowerScaling.kt`:

- Both hit-chance clamps — the enemy's **10% floor** and the player's **15% floor**
- The max-hit formula, with Kotlin's integer truncation preserved
- Uniform `0..maxHit` damage rolls, so landed hits can still do zero
- The enemy's independent 2.4-second attack clock, separate from your weapon speed
- The death check that runs **before** the eating phase, so food can't revive you
- The auto-eat loop, its eat threshold, and the **300-item-per-floor cap**
- The `freshlyKilled` carryover rule that makes the enemy type sticky across minutes
- Floor scaling above 100: HP to 10×, attack and defence *bonuses* to 1.3×

Enemy, equipment and food values are read from the game's own JSON assets (v1.14.0).

## Two things worth knowing

**Defence saturates.** Because the enemy's hit chance bottoms out at 10%, once your
effective Defence passes **5× a tier's effective attack**, more armour changes nothing at
all. For floors 61–80 that ceiling is 410.

**Food is usually the real wall.** Below the f81-100 tier a floor costs you almost nothing;
at floor 82 the damage jumps roughly sixfold. The calculator flags every floor where the
300-item cap binds — on those, more armour helps and more fish cannot.

Melee only (Attack/Strength style). Ranged and magic use different accuracy stats and
aren't ported.

## Licence

GPL-3.0, inherited from Idle Fantasy, from which the combat algorithm and game data are
derived. See `LICENSE`.

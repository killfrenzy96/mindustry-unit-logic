# Mindustry Unit Logic
This logic was created over my time playing Mindustry, designed for v8. It provides automation for many unit actions, such as attacking enemy units, repairing buildings, mining, and attacking enemy bases. Most importantly, pre-cautions are taken to increase the survivability of units while performing automated actions.

Additionally, there is some level of player control. Units will move to where you shoot. Taking direct control one of the units will make them enter formation with you, moving and shooting alongside you.

When the units have nothing to do, they will idle in range of a repair turret. If you have none, they will idle around the core instead - though I highly recommend you build a repair turret so that units can heal up.

Schematics for these scripts are available on the Steam Workshop: https://steamcommunity.com/id/killfrenzy96/myworkshopfiles/?appid=1127400&p=1

I may record a showcase if there's enough interest in my unit logic.

### Config
- `unitType`: The unit you want to bind and control. Do not change this unless the other unit is very similar (such as `@zenith`, `@antumbra`, and `@eclipse`).
- `bindLimit`: The amount of units to control, from `1` to `24`. Controlling too many units will cause delays. Controlling too few units may impact their awareness. Recommended `2` for microprocessors, `8` for logic processors, and `24` for hyper processors. You can build multiple processors to fill the unit limit. I recommend overdriving the processors to improve performance.
- `playerControl`: Allows the player to move the units by shooting. Note that this kind of like an attack-move. Units will prioritize attacking enemies as soon as they detect an enemy.
- `playerFormation`: When the player controls the same `unitType`, the unit enters formation with the player, copying their movement and attacks.
- `playerMixedFormation`: Allows `playerFormation` to activate even if the unit type does not match.
- `playerRetreat`: Makes the unit move out of the way of the player (only when the unit is idle).
- `safeAttackMode`: When `true`, units avoid attacking the enemy when they think it's too dangerous. Set this to `false` if you want your units to hunt down every detected enemy.
- `seekEnemyBase`: Seek out and attack the enemy base. Only activates with a full squad, when all units up to the `bindLimit` are alive. This prevents wiping out all the units to the enemy base, as well as providing strength in numbers. Units will bypass `spawnRange` and `turretRange` while attacking the enemy base.
- `spawnRange`: The distance units should stay away from spawn. You should decrease this if the map has a small spawn size.
- `turretRange`: The distance units should stay away from enemy turrets. You might want to increase this to at least `65` if the Foreshadow is a risk to you.
- `coreRepairTarget`: Prioritize repairing the core if it falls below a HP threshold, in percentage from `0` to `1`.
- `focusOnMining`: Makes mining ores more important than repairing or finding enemies to attack.
- `oreFillTarget`: How much ore the unit should mine before it stops mining, in percentage from `0` to `1`.
- `retreatHealth`: The health threshold before the unit should reteat for repairs, in percentage from `0` to `1`.
- `exploreDistance`: The maximum distance the unit should travel away from the core.

### General behaviour priority
Behaviour priority may change depending on certain factors.
1. When damaged, retreat for repairs.
2. Repairs the core. (`coreRepairTarget`)
3. Attack enemies within range. Does its best to avoid dangerous situations. (`safeAttackMode`)
4. Enters formation with the player upon controlling a ship. (`playerFormation`)
5. Follow player shooting. (`playerControl`)
6. Uses various methods in an attempt to locate enemy units (via turrets and other units).
7. Repair damaged buildings.
8. Mine ores. (`oreFillTarget`)
9. Moves away from players. (`playerRetreat`)
10. Idles on top of a repair turret.

### Caveats
- The code for each unit is not identical. Customizations had been applied to make it work for specific units. Only change the `unitType` for units that are very similar with each other - that is, they attack, move, mine, and repair in the same way.
- My unit logic is generally friendly with other unit logic. That is, it will never control a unit that's already binded to another processor, and will only control units up to the `bindLimit`. It will however, bind any free units to reach its binding limit.
- This has never been tested in PvP, nor have I ever tried PvP. I mainly use my unit logic for custom games and the campaign, including attack maps. There's probably behaviours that can be exploited by other players.
- I've included my unit cargo balancer logic. It can also be used for one-way item transfers. Despite the code complexity, steps were taken to ensure that it is performant. Units will not spend a long time waiting at each container, as long as you're not using a weak micro processor with a high `bindLimit`.

# Warfare Framework

![Game](https://img.shields.io/badge/game-Hearts%20of%20Iron%20IV-2f6f8f?style=flat-square)
![Supported version](https://img.shields.io/badge/supported%20version-1.19.2-2f6f8f?style=flat-square)
![Script](https://img.shields.io/badge/script-Clausewitz-2f6f8f?style=flat-square)
![Localisation](https://img.shields.io/badge/localisation-English-2f6f8f?style=flat-square)

Warfare Framework is a Hearts of Iron IV balance mod for the 1.19 game line. It reshapes infantry roles, artillery progression, terrain combat modifiers, selected air and naval defines, and a small set of support-company and AI-template rules.

The mod is intentionally direct: most changes are made through readable overrides in `common/`, with localisation and interface bindings kept in their standard HOI4 folders.

## Contents

- [Version and Testing Status](#version-and-testing-status)
- [Compatibility](#compatibility)
- [Installation](#installation)
- [Design Direction](#design-direction)
- [Gameplay Changes](#gameplay-changes)
- [Battalions](#battalions)
- [Artillery Equipment](#artillery-equipment)
- [Terrain](#terrain)
- [Air, Navy, and Defines](#air-navy-and-defines)
- [AI Templates](#ai-templates)
- [Known Issues / Current Limitations](#known-issues--current-limitations)
- [Project Layout](#project-layout)
- [Development Notes](#development-notes)

## Version and Testing Status

Warfare Framework has been tested and adjusted for **Hearts of Iron IV 1.19.2 Regular Update**.

Other game versions are not guaranteed to behave the same way. Later patches may change defines, technology layout, AI behaviour, or terrain handling, so compatibility should be rechecked after any HOI4 update.

## Compatibility

| Item | Value |
| --- | --- |
| Game | Hearts of Iron IV |
| Supported version | `1.19.*` |
| Mod name | `Warfare Framework` |
| Terrain handling | `replace_path="common/terrain"` |
| Known compatibility | Vanilla HOI4 1.19.2 is the tested baseline. Other mod combinations are unverified unless tested in-game, especially mods that edit terrain, units, equipment, technologies, defines, AI templates, or localisation. |

## Installation

Place the mod folder in the standard Hearts of Iron IV mod directory, then enable it from the Paradox launcher.

Typical user mod location on Windows:

```txt
Documents/Paradox Interactive/Hearts of Iron IV/mod/
```

The mod folder contains `descriptor.mod`. A launcher `.mod` entry should point to the mod folder and use:

```txt
name="Warfare Framework"
supported_version="1.19.*"
replace_path="common/terrain"
```

After changing units, equipment, terrain, defines, localisation, or interface files, restart the game before testing.

## Design Direction

Warfare Framework keeps the land-combat roster compact and distinct:

- Line Infantry is the standard backbone unit.
- Light Infantry is cheaper, faster, and easier to supply, but weaker in combat.
- Elite Infantry is stronger and gains combat XP faster, but is more expensive and slower to train.
- Field Artillery is the normal line-artillery option.
- Heavy Artillery is slower, supply-hungry assault artillery with stronger attack modifiers.
- Terrain and supply penalties are softened to reduce extreme combat cliffs.
- Naval dominance and air detection are tuned through defines rather than scripted events.

## Gameplay Changes

### Vanilla Unit Overrides

- Vanilla `infantry` remains active, but is renamed and rebalanced as Line Infantry.
- Vanilla `artillery_brigade` is disabled and zeroed out.
- Several vanilla infantry-like battalions are disabled in `common/units/infantry.txt`, including marines, mountaineers, rangers, paratroopers, motorized, penal, irregular, militia, and related variants.
- Mechanized remains active.
- Motorized artillery, rocket artillery, and motorized rocket artillery remain active.

### Localised Names

| Internal key | Display name |
| --- | --- |
| `infantry` | Line Infantry |
| `ww_light_infantry_battalion` | Light Infantry |
| `ww_elite_infantry_battalion` | Elite Infantry |
| `ww_field_artillery_battalion` | Field Artillery |
| `ww_heavy_assault_artillery_battalion` | Heavy Artillery |
| `artillery_equipment_4` | Late Artillery |

## Battalions

These are battalion-level modifiers and costs as currently implemented.

| Battalion | Width | Org | HP | Recovery | Soft | Hard | Defense | Breakthrough | Piercing | Speed | Supply | Weight | Manpower | Training | Equipment |
| --- | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | --- |
| Line Infantry | 2 | 60 | 25 | 0.30 | 0 | 0 | 0 | 0.333 | 0.25 | -0.05 | 0.070 | 0.50 | 1000 | 90 | 100 infantry equipment |
| Light Infantry | 2 | 65 | 21 | 0.35 | -0.167 | -0.5 | -0.182 | 0.667 | 0 | 0.10 | 0.045 | 0.35 | 850 | 100 | 85 infantry equipment |
| Elite Infantry | 2 | 75 | 30 | 0.65 | 0.667 | 0.5 | 0.364 | 3.667 | 0.5 | 0.05 | 0.075 | 0.45 | 1000 | 210 | 160 infantry equipment, 35 support equipment |
| Field Artillery | 3 | 25 | 8 | 0.10 | 0.10 | 0 | 0.05 | -0.10 | 0 | -0.10 | 0.180 | 0.80 | 500 | 120 | 36 artillery equipment |
| Heavy Artillery | 3 | 23.75 | 8 | 0.10 | 0.35 | 0.35 | -0.10 | 0.60 | 0.35 | -0.25 | 0.234 | 0.80 | 500 | 156 | 54 artillery equipment |

Elite Infantry also has:

```txt
experience_gain_army_unit_factor = 0.65
experience_loss_factor = -0.00556
```

The XP-loss value is tuned so a 9-battalion elite formation reaches roughly -5% total XP loss.

## Artillery Equipment

`common/units/equipment/ww_artillery_equipment.txt` overrides the artillery equipment line.

| Year | Equipment key | Soft | Hard | Piercing | Defense | Breakthrough | IC cost | Resources |
| --- | --- | ---: | ---: | ---: | ---: | ---: | ---: | --- |
| 1936 | `artillery_equipment_1` / archetype | 34 | 16 | 24 | 12 | 5 | 3.675 | 1 tungsten, 2 steel |
| 1939 | `artillery_equipment_2` | 40 | 22 | 34 | 14 | 6 | 4.4 | 1 tungsten, 2 steel |
| 1942 | `artillery_equipment_3` | 48 | 30 | 48 | 16 | 7 | 5.4 | 1 tungsten, 3 steel |
| 1944 | `artillery_equipment_4` | 56 | 38 | 58 | 18 | 8 | 6.25 | 2 tungsten, 3 steel |

Late Artillery is enabled by `common/technologies/ww_late_artillery.txt` through `artillery5`.

## Terrain

The mod fully overrides `common/terrain` via `replace_path="common/terrain"`. The current land-combat terrain modifiers are:

| Terrain | Attack | Defence |
| --- | ---: | ---: |
| Forest | -0.12 | 0.05 |
| Hills | -0.10 | 0.06 |
| Urban | -0.18 | 0.10 |
| Jungle | -0.22 | 0.08 |
| Marsh | -0.25 | 0.10 |
| Mountain | -0.30 | 0.12 |

Other terrain properties are preserved from the copied vanilla `00_terrain.txt` unless explicitly changed in the mod file.

## Air, Navy, and Defines

Defines are kept in `common/defines/ww_air_balance.lua`. The filename is historical; it now holds air, military, navy, intel, and country-level balance defines.

### Air and Supply

| Define | Value | Vanilla |
| --- | ---: | ---: |
| `AIR_WING_XP_TRAINING_MISSION_GAIN_DAILY` | 14.0 | 7.0 |
| `COMBAT_BETTER_AGILITY_DAMAGE_REDUCTION` | 0.75 | 0.45 |
| `DETECT_CHANCE_FROM_AIRCRAFTS` | 0.80 | 0.8 |
| `DETECT_CHANCE_FROM_RADARS` | 0.15 | 0.5 |
| `DETECT_CHANCE_FROM_OCCUPATION` | 0.05 | 0.10 |
| `AIR_SUPPLY_CONVERSION_SCALE` | 0.05 | 0.01 |

Transport planes are overridden with:

```txt
fuel_consumption = 0.65
```

This is a -35% fuel consumption change from vanilla 1.0.

A hidden global idea, `ww_global_air_mission_efficiency`, is applied to every existing country on startup:

```txt
air_mission_efficiency = 0.25
air_detection = 0.15
```

### Military Defines

| Define | Value | Vanilla |
| --- | ---: | ---: |
| `COMBAT_SUPPLY_LACK_ATTACKER_ATTACK` | -0.02 | -0.25 |
| `COMBAT_SUPPLY_LACK_ATTACKER_DEFEND` | -0.02 | -0.65 |
| `COMBAT_SUPPLY_LACK_DEFENDER_ATTACK` | -0.02 | -0.35 |
| `COMBAT_SUPPLY_LACK_DEFENDER_DEFEND` | -0.02 | -0.15 |
| `COMBAT_REINFORCE_CHANCE` | 0.50 | vanilla default |
| `COMBAT_OVER_WIDTH_PENALTY` | -0.75 | -1 |
| `COMBAT_OVER_WIDTH_PENALTY_MAX` | -0.45 | -0.33 |

### Navy and Intel Defines

| Define | Value | Vanilla |
| --- | ---: | ---: |
| `DOMINANCE_CONTROLLED_THRESHOLD_RATIO` | 0.10 | 0.60 |
| `DOMINANCE_DAILY_GAIN_FACTOR` | 0.04 | 0.02 |
| `DOMINANCE_DAILY_LOSS_FACTOR` | 0.03 | 0.04 |
| `NAVAL_DOMINANCE_STRIKE_FORCE_FRACTION` | 0.0006 | 0.0006 |
| `NAVAL_DOMINANCE_ORG_RECOVERY` | 0.20 | 0.10 |
| `NAVAL_DOMINANCE_SHIP_RECOVERY_CHANCE` | 0.25 | 0.10 |
| `AIR_BASE_DOMINANCE_FACTOR` | 0.005 | 0.02 |
| `RADAR_DOMINANCE_FACTOR` | 0.01 | 0.05 |
| `NAVAL_BASE_DOMINANCE_FACTOR` | 0.003 | 0.01 |
| `NAVAL_HOMEBASE_OWNERSHIP_BONUS` | 0.01 | 0.04 |
| `SHIP_SUPPORT_NEED_FACTOR` | 0.02 | 0.1 |
| `CONVOY_BLOCKED_BY_ENEMY_CONTROLLED_REGION` | false | true |
| `NAVAL_DOMINANCE_INTEL_LOW` | 0.25 | 0.4 |
| `NAVAL_DOMINANCE_INTEL_LOW_DOMINANCE_PENALTY_START` | 0.05 | 0.1 |
| `NAVAL_DOMINANCE_INTEL_LOW_DOMINANCE_MIN_PENALTY` | 0.80 | 0.5 |

Mission dominance ratios are overridden with the full array so mission indexes remain intact:

```lua
NDefines.NNavy.MISSION_DOMINANCE_RATIOS = {
    0.0, -- HOLD
    1.5, -- PATROL
    0.8, -- STRIKE FORCE
    0.5, -- CONVOY RAIDING
    0.7, -- CONVOY ESCORT
    0.3, -- MINES PLANTING
    0.3, -- MINES SWEEPING
    0.0, -- TRAIN
    0.0, -- RESERVE_FLEET
    2.0, -- NAVAL_INVASION_SUPPORT
}
```


## Signal Company Initiative

Signal Company initiative progression is implemented through `common/units/signal.txt` and `common/technologies/ww_signal_company.txt`.

| Level | Total initiative |
| --- | ---: |
| Signal Company I | 0.20 |
| Signal Company II | 0.35 |
| Signal Company III | 0.50 |
| Signal Company IV | 0.70 |

Implementation:

```txt
signal_company base initiative = 0.20
tech_signal_company2 adds 0.15
tech_signal_company3 adds 0.15
tech_signal_company4 adds 0.20
```

## AI Templates

AI templates are defined in `common/ai_templates/ww_units_generic.txt`.

| Template | Weight | Support | Line battalions |
| --- | ---: | --- | --- |
| Light Infantry | 1.0 | engineer, recon, artillery | 9 Light Infantry, 1 Field Artillery |
| Elite Infantry | 0.20 | engineer | 6 Line Infantry, 2 Elite Infantry, 2 Field Artillery, 1 Heavy Artillery |

The intent is for AI countries to remain mostly line/light infantry weighted while using elite formations sparingly. Heavy Artillery appears only in the low-weight elite template.

## Known Issues / Current Limitations

- Naval invasion and naval dominance flow is still experimental. The mod reduces hard-lock behavior, but extreme cases such as direct English Channel invasions may still need more testing.
- `CONVOY_BLOCKED_BY_ENEMY_CONTROLLED_REGION = false` may affect general convoy routing beyond naval invasions. This is intentional for now, but needs longer campaign testing.
- Tank balance has not received a dedicated pass yet. Tanks may still feel too efficient or too frustrating in some situations.
- Air Attack module economy has not received a full designer pass yet. Current air changes mainly rebalance detection, mission efficiency, and agility.
- Special-force battalions are currently disabled as part of the infantry-role redesign. Future special-force behavior may be represented through training or support systems instead of separate battalion types.
- Current balance is primarily tested against AI, not multiplayer.
- The mod fully overrides `common/terrain`, so compatibility with other terrain-changing mods is limited.
- Compatibility with other mods has not been comprehensively tested. Treat mixed load orders as unsupported unless verified in-game.
- AI templates are functional but still conservative. AI usage of Light Infantry, Elite Infantry, and Heavy Artillery may need more campaign testing.
## Project Layout

| Path | Purpose |
| --- | --- |
| `common/units/infantry.txt` | Line Infantry override and disabled vanilla infantry-like battalions |
| `common/units/ww_combat_battalions.txt` | Custom combat battalions |
| `common/units/artillery_brigade.txt` | Vanilla line artillery disablement |
| `common/units/equipment/ww_artillery_equipment.txt` | Artillery equipment progression |
| `common/units/equipment/ww_transport_plane_equipment.txt` | Transport plane fuel override |
| `common/technologies/ww_late_artillery.txt` | Late Artillery unlock |
| `common/terrain/00_terrain.txt` | Full terrain override |
| `common/defines/ww_air_balance.lua` | Air, military, navy, intel, and supply defines |
| `common/ideas/ww_units_hidden_ideas.txt` | Hidden global air modifier idea |
| `common/on_actions/ww_units_on_actions.txt` | Startup application of hidden global idea |
| `common/ai_templates/ww_units_generic.txt` | AI division template targets |
| `interface/ww_subuniticons.gfx` | Unit icon bindings |
| `localisation/english/` | Unit and equipment names |

## Development Notes

- Keep script files as UTF-8 without BOM. HOI4 can fail to read some script files when a BOM is present.
- Localisation `.yml` files may use BOM if needed by the localisation pipeline.
- Do not edit vanilla game files directly; copy and override inside the mod folder.
- Keep `replace_path="common/terrain"` in both launcher metadata and `descriptor.mod` while terrain is fully overridden.
- Do not restore `interface/mapicons.gui`; it previously caused strategic-view/map-icon UI errors.
- When editing arrays such as `MISSION_DOMINANCE_RATIOS`, keep the full vanilla order and override the whole array. A partial array can shift mission indexes and break behaviour.
- If a unit designer value looks unexpected, check both battalion modifiers and equipment stats. HOI4 combines both when displaying final division stats.


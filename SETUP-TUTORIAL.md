# Legendary Rotations HMI — Setup Tutorial

## What is the HMI?

The **HMI** (Human–Machine Interface) is a configuration window that opens alongside your rotation — think of it like **Hekili**, but with full control over every spell, condition, and priority. Where Hekili shows you *what* to press next, the Legendary HMI lets you define *how* the rotation decides what to press, with a familiar grid-based editor for spell priorities, conditions, cooldown management, timing delays, and profiles.

> **Auto-Close Behavior:** By default, the HMI automatically closes itself if no rotation tick has fired for **60 seconds** (e.g., you stopped the bot, logged out, or disconnected). This is normal — it saves your config before closing. You can disable this via the **"Close HMI after 1 minute out of game"** checkbox on the General tab.

---

## Table of Contents

1. [First Launch & Initial Setup](#1-first-launch--initial-setup)
2. [HMI Window Overview](#2-hmi-window-overview)
3. [General Tab](#3-general-tab)
4. [Timing Tab](#4-timing-tab)
5. [Rotation Tab — Categories & Sub-Tabs](#5-rotation-tab--categories--sub-tabs)
6. [Understanding the Spell Grid](#6-understanding-the-spell-grid)
7. [The Condition System](#7-the-condition-system)
8. [Major Cooldowns & SaveCDs Toggle](#8-major-cooldowns--savecds-toggle)
9. [Profiles — Save, Load, Export & Import](#9-profiles--save-load-export--import)
10. [In-Game Slash Commands](#10-in-game-slash-commands)
11. [Assisted Highlight Mode vs Full Auto](#11-assisted-highlight-mode-vs-full-auto)
12. [Tips & Best Practices](#12-tips--best-practices)
13. [Condition Reference Sheet](#13-condition-reference-sheet)

---

## 1. First Launch & Initial Setup

### Prerequisites

- **Inferno Bot** installed and running.
- A **rotation .cs file** for your class placed in the correct Inferno rotation folder.

### Start-Up Sequence

1. **Start Inferno Loader before b.net and WoW.** 
2. Log into World of Warcraft.
3. Select your rotation file, and click Start.
4. Type `/reload` in the game chat. This loads the bot's internal macros and keybinds automatically.
5. The **HMI window** opens automatically once the rotation initializes.

> **Important:** The bot reserves `Numpad 0-9`, `F1-F12`, `[`, and `]` keys internally. Do **not** bind those to anything in WoW.

### Spellbook Limit

The Inferno API has a **50-slot spellbook limit**. The rotation automatically registers your spec's spells, shared spells, and racial spells at startup. If your combined spell count exceeds 50, lower-priority spells may be excluded.

---

## 2. HMI Window Overview

The HMI opens as a resizable WinForms window (default 900×700 pixels, minimum 600×400).

### Main Tabs

| Tab | Purpose |
|-----|---------|
| **General** | Global rotation settings (mode, targeting, cooldown threshold) |
| **Timing** | Cast and kick delay configuration with randomization |
| **Rotation** | The main tab — contains all spell categories as sub-tabs |
| **Profiles** | Save, load, delete, export, and import rotation profiles |

### Footer Bar

At the bottom of the **General tab** you'll find:

- **Pause Button** — Shows `▶ ROTATION ACTIVE` (green) or `⏸ ROTATION PAUSED` (red). Click to pause/resume.
- **Cooldown Status** — Shows `Cooldowns: ON` (green) or `Cooldowns: OFF` (red), reflecting the in-game SaveCDs toggle state.

---

## 3. General Tab

The General tab controls how the rotation behaves at a high level.

| Setting | Description | Default |
|---------|-------------|---------|
| **Use Assisted Highlight Priority** | When enabled, the rotation only handles non-damage tasks (kicks, defensives, dispels, items, healing, utility, major CDs). Damage spells are left to your WeakAura/addon highlight. When disabled, the rotation runs everything in full-auto mode. | Off |
| **Enable Auto Target** | Automatically switches to a nearby enemy target if your current target dies or is invalid. | On |
| **Start Combat from Out of Combat** | Allows the rotation to fire abilities and start combat when you are out of combat (e.g., pulling). | Off |
| **HMI Top Most** | Keeps the HMI window on top of all other windows. | Off |
| **Close HMI after 1 minute out of game** | Automatically closes the HMI window if no rotation tick has fired for 60 seconds (e.g., you stopped the bot). Config is saved before closing. | On |
| **Major CD – Target Time To Die (s)** | If the target is estimated to die within this many seconds, major cooldowns are skipped to avoid waste. Set to `0` to disable. Range: 0–60. | 10 |
| **Reset to Defaults** | Resets ALL settings (general, timing, and rotation lines) back to factory defaults. Requires confirmation. | — |

---

## 4. Timing Tab

The Timing tab controls how quickly the bot reacts. Adjusting these values adds humanization to your gameplay.

### Cast Delay

| Setting | Description | Range | Default |
|---------|-------------|-------|---------|
| **Enable Cast Delay** | Master toggle for the cast delay system. When **unchecked**, spells fire immediately with no artificial delay. Reset-to-Defaults turns this **On**. | — | Off |
| **Cast Delay Min (ms)** | Minimum delay before casting the next spell. | 0–5000 | 10 |
| **Cast Delay Max (ms)** | Maximum delay before casting the next spell. | 0–5000 | 50 |
| **Randomize Cast Delay** | When checked, the delay is randomized between Min and Max each cast. When unchecked, Min is always used. | — | On |

### Kick Delay

| Setting | Description | Range | Default |
|---------|-------------|-------|---------|
| **Kick Delay Min (ms)** | Minimum delay before interrupting an enemy cast. | 0–5000 | 0 |
| **Kick Delay Max (ms)** | Maximum delay before interrupting an enemy cast. | 0–5000 | 200 |
| **Randomize Kick Delay** | When checked, the kick delay is randomized between Min and Max. | — | Off |

> **Tip:** Higher delays look more human but may cause you to miss short casts. A good starting point is 100–400ms for casts and 200–600ms for kicks.

---

## 5. Rotation Tab — Categories & Sub-Tabs

The **Rotation** tab is where the real customization happens. It contains multiple sub-tabs, each representing a category of spells. Categories are evaluated in a specific priority order during combat.

### Sub-Tab Categories

| Category | Purpose |
|----------|---------|
| **OOC** | Out-of-Combat actions (buffs, food, mount-up macros) |
| **Kicks** | Interrupt spells (e.g., Counterspell, Pummel, Spell Lock) |
| **Defensives** | Defensive cooldowns (e.g., Unending Resolve, Dark Pact) |
| **Items** | Trinkets, weapon, and consumables (TopTrinket, BottomTrinket, UseWeapon, Item1–Item5) |
| **Dispels** | Dispel/cleanse abilities |
| **Racials** | Racial abilities (Blood Fury, Berserking, etc.) |
| **Healing** | Healing spells (primarily for healer specs) |
| **Utility** | Utility spells (movement, crowd control) |
| **PvP** | PvP-specific abilities (class-specific — only present for some classes) |
| **Damage_Opener** | Opener/burst sequence (executed first in combat) |
| **Damage_SingleTarget** | Single-target damage rotation |
| **Damage_AoE** | AoE damage rotation (activated when enemy count ≥ AoE threshold) |

### Category-Specific Controls

Some categories have extra controls at the top of their tab:

- **Damage_AoE Tab:**
  - `AoE Threshold` (1–10) — Minimum number of enemies required to switch to AoE rotation.
  - `Use EnemiesInRange` checkbox — When checked, counts enemies at 40 yards instead of melee (8 yards).

- **Items Tab:**
  - Trinket macros (`TopTrinket`, `BottomTrinket`) and weapon macro (`UseWeapon`) target equipment slots 13, 14, and 16 respectively.
  - Certain trinkets are engine-blacklisted (e.g., Radiant Plume, Umbral Plume) and will never be used on-use, even if conditions are met.

- **Kicks Tab:**
  - `Kick Cast Remaining (ms)` (100–5000) — Only interrupt when the enemy cast has this much time left.
  - `Kick Channel Elapsed (ms)` (0–5000) — Only interrupt a channel after this much time has elapsed.

- **Dispels Tab:**
  - Six type checkboxes: `Magic`, `Poison`, `Disease`, `Curse`, `Bleed`, `Snare` — Toggle which debuff types to auto-dispel.

### Enable Checkbox

Every category tab has an **"Enable [Category]"** checkbox at the top left. When **unchecked**, the entire category is skipped during execution — no spells from that category will fire.

---

## 6. Understanding the Spell Grid

Each category tab displays a **DataGridView** (spreadsheet-like table) with the following columns:

| Column | Type | Description |
|--------|------|-------------|
| **✓ (Enabled)** | Checkbox | Per-spell toggle. Uncheck to disable a single spell without deleting it. |
| **Spell Name** | Text (read-only) | The spell or macro name (e.g., "Shadow Bolt", "TopTrinket"). |
| **Item Name** | Text (Items tab only) | The in-game item name for consumable macros (e.g., "Algari Healing Potion"). |
| **Spell Order** | Number (read-only) | Execution priority. **Lower number = fires first.** Lines are evaluated top-to-bottom in priority order. |
| **Condition** | Text (read-only) | The condition expression that must evaluate to `true` for this spell to fire. |
| **CD** | Checkbox | Marks this spell as a **Major Cooldown**. Major CDs are subject to the SaveCDs toggle and Target Time-To-Die threshold. |

### How the Grid Works

1. Every combat tick (~100ms), the engine iterates through enabled categories.
2. Within each category, spell lines are sorted by **Spell Order** (ascending).
3. For each line: if **Enabled** is checked, the **Condition** evaluates to true, and the spell can be cast (off cooldown, in range, etc.), the spell fires.
4. After a spell fires, the engine moves to the next category (cascade behavior).

### Editing Spells

- **You cannot add or remove rows directly** — the grid is populated from seed defaults or loaded profiles.
- **You can edit conditions** by clicking the Condition cell and typing a new expression.
- **You can toggle Enabled/CD** checkboxes by clicking them.
- **Changes auto-save** — every edit immediately writes to the config file.

### Default Seeds

When a category has no saved configuration, the rotation auto-populates it with **seed defaults** — a recommended spell order and condition set for your spec. These seeds are curated per class and specialization. You can freely modify them afterward.

Once you change a profile, those edits are now the saved version of that profile. The rotation does **not** automatically re-seed that category every time you open the HMI.

### Getting the Default Seeds Back

If you want the shipped defaults again after editing something, use one of these options:

1. **Load a clean profile** if you still have one that was never edited (for example an untouched `Default` profile).
2. **Use General → Reset to Defaults** if you want to wipe the current setup and rebuild it from the current default seeds.
3. **Save or export your current setup first** if you may want to go back to it later, then reset/load the defaults.

> **Important:** Seed defaults only auto-populate empty configuration. If you want the newest default lines back, you must load a clean profile or reset the current one.

---

## 7. The Condition System

Conditions are the heart of the rotation. They control **when** each spell fires.

### Operators

| Operator | Meaning | Example |
|----------|---------|---------|
| `OR` or `\|\|` | Logical OR — either side can be true | `Player.Health < 50 OR HasBuff(Enrage)` |
| `AND` or `&&` | Logical AND — both sides must be true | `InCombat(player) AND EnemiesInMelee >= 3` |
| `NOT` | Negation — inverts the result | `NOT HasBuff(Stealth,player)` |
| `(` `)` | Grouping — control evaluation order | `(A OR B) AND C` |

**Precedence** (highest to lowest): `NOT` → `AND` → `OR`

### Comparison Operators

Used inside condition functions:

| Operator | Meaning |
|----------|---------|
| `>=` | Greater than or equal to |
| `<=` | Less than or equal to |
| `>` | Greater than |
| `<` | Less than |
| `==` | Equal to |
| `!=` | Not equal to |

### Common Condition Examples

```
# Always cast (no condition needed — leave the field empty)

# Cast when player health is below 50%
Player.Health < 50

# Cast when target has a specific debuff
HasDebuff(Agony,target)

# Cast when buff stacks reach a threshold
BuffStacks(Maelstrom Weapon,player) >= 5

# Cast only if buff is missing (refresh)
NOT HasBuff(Flame Shock,target)

# Complex: Cast when moving OR when an instant-cast proc is active
IsPlayerMoving OR HasBuff(Predator's Swiftness,player)

# AoE: Cast when 3+ enemies in melee range
EnemiesInMelee >= 3

# Opener: Cast in first 8 seconds of combat
CombatTime < 8000

# Boss only: Cast only on bosses
IsBoss(target)

# Group healing: Cast when group average health drops
GroupMeanHealthCheck(85, 3, HealingSpell)

# Target lowest HP party member and then heal
GroupLowestHealth(80)

# Major cooldown gate (handled automatically by the CD checkbox, but can also be written)
MajorCooldowns

# Cooldown-based: Cast when another spell is on cooldown
SpellCooldown(Metamorphosis) > 10000

# Talent check: Only cast if talent is learned
IsSpellKnown(12345)

# Cast on a specific unit
CastOn(focus)
CastOn(tank)
CastOn(mouseover)

# Ground-targeted spells
Placement(@Player)
Placement(@Cursor)

# Spell upgrade (cast base spell instead of the line's spell)
Upgrade(Lightning Bolt)
```

---

## 8. Major Cooldowns & SaveCDs Toggle

### What is a Major Cooldown?

Any spell line with the **CD** checkbox checked is treated as a Major Cooldown. These are typically high-impact, long-cooldown abilities (e.g., Metamorphosis, Dark Soul, Bloodlust).

### SaveCDs Toggle

In game, you can toggle cooldowns on/off using a slash command:

```
/xxxxx SaveCDs
```

(Replace `xxxxx` with the first 5 lowercase letters of your addon name.)

When **SaveCDs is ON** (cooldowns disabled):
- All Major CD lines are **skipped** entirely.
- The footer shows `Cooldowns: OFF` in red.
- Useful for saving CDs for an upcoming boss phase or mechanic.

When **SaveCDs is OFF** (cooldowns enabled, the default):
- Major CD lines fire normally if their conditions are met.
- The footer shows `Cooldowns: ON` in green.

### Target Time-To-Die

In addition to the manual toggle, the rotation automatically **saves cooldowns near the end of a fight**. If the estimated target time-to-die is less than the threshold (default: 10 seconds), Major CDs are skipped to avoid waste.

The estimator works by:
1. Sampling target HP every ~1 second.
2. Computing a smoothed HP-per-second damage rate.
3. Dividing current HP by that rate to estimate seconds remaining.

You can adjust or disable this in **General Tab → Major CD – Target Time To Die (s)**. Set to `0` to disable.

### The "MajorCooldowns" Condition Keyword

You can also use `MajorCooldowns` as a condition string on any line. It evaluates to `true` when SaveCDs is OFF (i.e., cooldowns should fire). This is an alternative to using the CD checkbox — both achieve the same goal.

---

## 9. Profiles — Save, Load, Export & Import

Profiles let you store multiple rotation configurations and switch between them.

### Profile Storage

Profiles are stored on disk in:
```
Legendary_HMI_Profiles/Retail_{Class}_{Spec}_{ProfileName}/
```

Each profile folder contains:
- `config.json` — All rotation settings, spell lines, conditions, and toggles.
- `metadata.txt` — Profile name, description, creation date, last modified date.

### Profile Tab Controls

| Button | Action |
|--------|--------|
| **Load** | Loads the selected profile, replacing all current settings and rotation lines. |
| **Save Current** | Overwrites the currently active profile with the current in-memory settings. |
| **Save As New** | Creates a brand-new profile from the current settings. Prompts for a name and description. |
| **Edit** | Rename a profile or update its description. |
| **Delete** | Permanently deletes a profile. The "Default" profile cannot be deleted. |
| **Refresh** | Reloads the profile list from disk. |
| **Export** | Saves the current profile's `config.json` to any location (e.g., to share with others). |
| **Import** | Loads a `config.json` file from disk and creates a new profile from it. |

### Profile Grid Columns

| Column | Description |
|--------|-------------|
| **Profile Name** | The display name of the profile. |
| **Description** | Optional user description. |
| **Last Modified** | Timestamp of the last save. |
| **Default** | Indicates if this is the default profile. |

### Workflow Example

1. Start with the default seed configuration.
2. Customize spells and conditions for your playstyle.
3. Click **Save As New** → name it "Mythic+ Build".
4. Switch specs or change talents, modify the rotation.
5. Click **Save As New** → name it "Raid Build".
6. Now you can **Load** either profile at any time to switch configurations instantly.

### How to Return to Defaults Later

If you already changed a profile and want the original seeded setup again:

1. Click **Save As New** first if you want to keep your current edited version.
2. Load a clean defaults-based profile if you still have one.
3. If every profile has already been edited, use **Reset to Defaults** on the General tab to rebuild the current setup from the shipped defaults.

---

## 10. In-Game Slash Commands

All slash commands start with the **first 5 lowercase letters of your addon name**. For example, if your addon is `LegendaryWarlock`, the prefix is `/legen`.

| Command | Description |
|---------|-------------|
| `/legen toggle` | Pause/unpause the rotation. |
| `/legen wait #` | Pause the rotation for `#` seconds, then auto-resume. Example: `/legen wait 3` |
| `/legen SaveCDs` | Toggle major cooldowns on/off. |
| `/legen SaveCDs #` | Toggle SaveCDs on, auto-disable after `#` seconds. |
| `/legen SaveAOE` | Toggle AoE routing on/off. When **on**, the rotation forces single-target even when AoE threshold is met. |

### Spell Queue — `/lq`

The rotation includes a built-in spell queue system for on-demand spell casting.

```
/lq <target> <SpellName>
```

| Target | Description |
|--------|-------------|
| `focus` | Cast on your focus target |
| `mouseover` | Cast on your mouseover target |
| `cursor` | Place ground-targeted ability at your cursor |

**Example:** `/lq focus Misdirection` queues Misdirection to be cast on your focus target on the next available tick.

Available queue spells are class-specific — each rotation registers its own set of queueable abilities (e.g., interrupts on focus, ground-targeted traps at cursor, utility on mouseover).

---

## 11. Assisted Highlight Mode vs Full Auto

The rotation supports two fundamental modes:

### Full Auto Mode (default)

When **"Use Assisted Highlight Priority"** is **unchecked**:

- The rotation handles **everything**: kicks, defensives, dispels, items, healing, utility, and all damage spells.
- Spells fire in category priority order every tick.
- Best for: players who want a fully hands-off rotation.

### Assisted Highlight Mode

When **"Use Assisted Highlight Priority"** is **checked**:

- The rotation handles: kicks, defensives, dispels, items, racials, healing, utility, and **only Major Cooldowns from damage tabs**.
- Damage spells (non-CD) are **not** fired by the bot — instead, your WeakAura or addon overlay highlights the recommended spell, and you press it manually.
- The engine fires Major CDs from `Damage_Opener` and the current damage category, then falls through to the addon's recommended spell.
- Best for: players who want automation for non-damage tasks but prefer pressing their own damage buttons (looks more natural, feels more interactive).

### Execution Priority (Full Auto)

```
Healing → Dispels → Defensives → OOC (if enabled) →
Items → Kicks → Racials → Utility →
Damage_Opener → Damage_SingleTarget OR Damage_AoE
```

### Execution Priority (Assisted Highlight)

```
Defensives → Items → Kicks → Dispels → Racials → Healing → Utility →
Major CDs from Damage_Opener → Major CDs from Damage_ST/AoE →
Fall through to addon-recommended spell
```

---

## 12. Tips & Best Practices

### Getting Started

1. **Start with defaults.** The seed configuration is a solid baseline for your spec. Try it before changing anything.
2. **Learn one category at a time.** Start by tweaking Damage conditions, then move to Kicks, then Defensives.
3. **Test in a training dummy area.** Open the HMI, make changes, and observe real-time behavior.

### Conditions

- **Leave the condition empty** for spells that should always be cast when ready (no gating).
- **Use `OR` liberally** for instant-cast buffs: `IsPlayerMoving OR HasBuff(Proc,player)`.
- **Time values are in milliseconds** — `CombatTime < 8000` means 8 seconds, `BuffRemaining(Hot,player) <= 5000` means ≤ 5 seconds remaining.
- **Use `NOT`** to invert: `NOT HasDebuff(FlameShock,target)` → cast only if debuff is missing.

### Performance

- **Disable unused categories.** If you never use Healing on a DPS spec, uncheck "Enable Healing" to save cycles.
- **Uncheck individual spells** you don't want instead of deleting them — you might want them back later.
- **Don't set kick delays too low** — sub-100ms kicks look non-human.

### Profiles

- **Create profiles for different content:** one for Mythic+, one for Raid, one for PvP.
- **Export before making big changes** — you can always import the backup.
- **The "Default" profile cannot be deleted** — it serves as your fallback.
- **If you want to test changes safely, use Save As New first** so you always have a clean defaults-based profile to load later.

### Major Cooldowns

- Mark **all big CDs** with the CD checkbox (Metamorphosis, Dark Soul, Ascendance, etc.).
- Keep Target Time-To-Die at **10s** for raids, consider lowering to **5s** or **0** for Mythic+ where trash dies fast but you still want CDs.
- Use `/legen SaveCDs` during intermissions or before a burst window to hold CDs.

---

## 13. Condition Reference Sheet

### Health & Resources

| Condition | Description |
|-----------|-------------|
| `Player.Health < 50` | Player health percent below 50% |
| `Health(target) < 30` | Target health percent below 30% |
| `Mana < 40` | Player mana percent below 40% |
| `Power(player,INDEX) >= 60` | Player power by type index (see below) |
| `Resource >= 85` | Primary resource (Soul Shards, Maelstrom, etc.) |

**Power Type Indices:** 0=Mana, 1=Rage, 2=Focus, 3=Energy, 4=ComboPoints, 5=Runes, 6=RunicPower, 7=SoulShards, 8=LunarPower, 9=HolyPower, 11=Maelstrom, 12=Chi, 13=Insanity, 16=ArcaneCharges, 17=Fury, 18=Pain, 19=Essence

### Buffs & Debuffs

| Condition | Description |
|-----------|-------------|
| `HasBuff(Name,unit)` | Unit has the named buff. Default unit: `player` |
| `HasBuff(Name,unit,false)` | Check buff regardless of who applied it |
| `BuffStacks(Name,unit) >= N` | Buff stack count on unit |
| `BuffRemaining(Name,unit) <= 5000` | Buff time remaining in ms |
| `HasDebuff(Name,unit)` | Unit has the named debuff. Default unit: `target` |
| `DebuffStacks(Name,unit) >= N` | Debuff stack count on unit |
| `DebuffRemaining(Name,unit) <= 5000` | Debuff time remaining in ms |

### Cooldowns & Casts

| Condition | Description |
|-----------|-------------|
| `SpellCooldown(Name) > 10000` | Spell cooldown remaining in ms |
| `SpellCharges(Name) >= 2` | Current charges for charge-based spells |
| `ChargesFractional(Name,VALUE) >= 1.5` | Fractional charges (e.g. 1.7). VALUE = recharge duration in ms from Wowhead |
| `CanCast(Name,unit,ignoreGCD)` | Full cast check (range, resources, GCD) |
| `IsSpellKnown(SpellID)` | Talent/spell is learned |
| `SpellInRange(Name,unit)` | True if the spell is in range of the unit |
| `CastingID(unit) == 12345` | Specific spell being cast by unit |
| `CastingRemaining(unit) <= 800` | Cast time remaining in ms |
| `IsChanneling(unit)` | Unit is channeling |
| `IsInterruptable(unit)` | Unit's cast can be interrupted |

### Combat & Encounter

| Condition | Description |
|-----------|-------------|
| `InCombat(player)` | Player is in combat |
| `CombatTime < 8000` | Time since combat started (ms) |
| `IsBoss(target)` | Target is a boss |
| `IsLusted()` | Bloodlust/Heroism is active or sated |
| `TargetIsEnemy` | Target can be attacked |
| `IsPlayerMoving` | Player is currently moving |
| `IsMovingFor(VALUE)` | True when player has been moving for at least VALUE ms |
| `HealthDropRate(unit) >= 10` | HP% lost per second for the unit (requires a few seconds of tracking) |

### Enemy & Ally Counts

| Condition | Description |
|-----------|-------------|
| `EnemiesInMelee >= 3` | Enemies within 8 yards |
| `EnemiesInRange >= 3` | Enemies within 40 yards |
| `EnemiesNearUnit(10,mouseover) >= 3` | Enemies within custom distance of unit |
| `FriendsNearUnit(10,tank) >= 2` | Allies within custom distance of unit |

### Group & Healing

| Condition | Description |
|-----------|-------------|
| `InParty()` | In a party (not raid) |
| `InRaid()` | In a raid |
| `GroupSize() >= 5` | Number of group members |
| `GroupLowestHealth(80)` | Finds & focuses lowest HP member below threshold |
| `GroupMeanHealthCheck(85,3,Spell)` | Average group HP check within the Spellrange of Spell |
| `GroupBuffCount(BuffID) >= 5` | Count members with specific buff |
| `GroupBuffMissing(Buff,Spell)` | Find member missing a buff |
| `FocusLowestMana(80)` | Focus lowest mana member |

### Targeting Directives

These always return `true` but set up targeting for the spell:

| Directive | Description |
|-----------|-------------|
| `CastOn(unit)` | Cast on specific unit (focus, tank, mouseover, etc.) |
| `Placement(@Player)` | Ground-target at player position |
| `Placement(@Cursor)` | Ground-target at cursor position |
| `Upgrade(SpellName)` | Cast a different base spell instead |
| `UnitByName(PlayerName)` | Target a specific player by name |
| `FocusMissingBuff(Buff)` | Find group member missing buff, set as focus |

### Special Checks

| Condition | Description |
|-----------|-------------|
| `MajorCooldowns` | Returns `true` when SaveCDs is OFF |
| `HasDispellableDebuff(unit)` | Unit has a dispellable debuff |
| `HasPurgeableBuff(unit)` | Unit has a purgeable buff |
| `HasEnrageBuff(unit)` | Unit has an enrage effect |
| `HasSnareDebuff(unit)` | Unit has a snare effect |
| `HasPet` | Player has a living pet |
| `HasDeadPet` | Player has a dead pet |
| `BossCast` | DBM/BigWigs alarm is active |
| `BossCastTankbuster` | Boss is casting a tankbuster |
| `BossCastTankSwap` | Boss is casting a tank-swap mechanic |
| `BossCastDanger` | Any dangerous boss cast active |
| `ItemCooldown(SLOT) == 0` | Trinket/item ready (13=Trinket1, 14=Trinket2, 16=Weapon) |
| `CanUseItem(SLOT)` | True if an inventory item slot is usable (has an active on-use ability and is off cooldown) |

---

## Quick-Start Checklist

- [ ] Start the Inferno loader **before** logging in
- [ ] Log into the game
- [ ] Start the Rotation with the HMI ending -> HMI window opens
- [ ] You will get "Addon Mismatch" from the loader
- [ ] Type `/reload` in chat
- [ ] The rotation should work now
- [ ] Review the **General** tab settings
- [ ] Go to the **Rotation** tab and browse each category's spells
- [ ] Adjust **Timing** tab delays to taste
- [ ] Test on a training dummy
- [ ] Customize conditions as needed
- [ ] Save your configuration as a new **Profile**
- [ ] Learn the `/legen toggle` and `/legen SaveCDs` slash commands

---

*Last updated: June 2025*

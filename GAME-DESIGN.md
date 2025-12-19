# Space Defense - Game Design Document

## Current Implementation (As-Built)

This document describes the game as currently implemented in index.html.

---

## Core Game Mechanics

### Player Ship
- Orbits Earth at fixed radius (70 pixels)
- Cannot move closer/farther from Earth
- Rotation controlled by Q (counter-clockwise) and A (clockwise)
- Ship grows visually with power-ups
- Ship color changes based on weapon level

### Objective
- Defend Earth from alien invasions
- Survive 20 waves of increasing difficulty
- Destroy aliens before they reach Earth
- Collect power-ups from bonus ships

### Lose Conditions
1. Player loses all lives (start with 3)
2. Earth health reaches 0 (start with 10)

### Win Condition
- Complete Wave 20

---

## Player Starting Stats

| Stat | Starting Value | Notes |
|------|----------------|-------|
| Lives | 3 | Max 5 with power-ups |
| Earth Health | 10 | Max 15 with power-ups |
| Rotation Speed | 0.035 rad/frame | Max 0.105 (10 upgrades) |
| Bullet Speed | 12.0 | FIXED - never changes |
| Fire Rate | 30 frames cooldown | Min 5 frames (5 upgrades) |
| Bullets per Shot | 1 | Max 5 with power-ups |
| Missiles | No | Unlocked via power-up |
| Orbit Radius | 70 pixels | Fixed |
| Ship Size | 12 pixels | Visual growth only |

---

## Status Bar Metrics (Sidebar)

### 1. WAVE (Yellow)
- Current wave/level (1-20)
- No power-up relation
- Auto-progresses

### 2. LIVES (Red)
- Player lives remaining (0-5)
- Power-up: "1UP" (Extra Life)
- Capped at 5 lives

### 3. EARTH HP (Blue)
- Earth's health (0-15)
- Starting: 10
- Power-up: "+HP" (Earth Health)

### 4. SPEED (Green)
- Ship rotation/orbit speed
- Range: 0.035 → 0.105
- Power-up: "SPD" (Speed) +0.007/upgrade
- Max 10 upgrades

### 5. FIREPOWER (Orange)
- Bullets fired simultaneously (1-5)
- Modes: Single → Double → Triple → Quad → Quint
- Power-ups: x2, x3, x4, x5

### 6. SHOTSPEED (Cyan)
- Fire rate (inverted: lower cooldown = faster)
- Range: 30 frames → 5 frames
- Power-up: "FR" (Fire Rate) -5 frames/upgrade
- Max 5 upgrades

### 7. SUPERPOWERS (Magenta)
- Active special abilities (0-3)
- Each ability = 33.33%
- Missiles, Auto-fire, Mirror Ship

---

## Power-Up System

All power-ups drop from cyan Bonus Ships (starting Level 2).

| Power-Up | Label | Color | Min Level | Effect | Max/Cap |
|----------|-------|-------|-----------|--------|---------|
| Speed | SPD | Green | 1 | +0.007 rotation speed | 10 upgrades (0.105) |
| Double | x2 | Yellow | 1 | 2 bullets per shot | One-time |
| Fire Rate | FR | Cyan | 1 | -5 frames cooldown | 5 upgrades (5 frames) |
| Earth Health | +HP | Blue | 1 | Restore/add Earth HP | 15 max |
| Triple | x3 | Orange | 6 | 3 bullets per shot | One-time |
| Missiles | MSL | Magenta | 7 | Homing missiles (O key) | One-time |
| Extra Life | 1UP | Green | 11 | +1 life | 5 lives max |
| Quadruple | x4 | Red-Orange | 12 | 4 bullets per shot | One-time |
| Mirror Ship | 2X | White | 12 | 2nd ship opposite side (30s) | Temporary |
| Screen Clear | BOOM | Orange | 13 | Destroy all enemies (B key) | One-time use |
| Quintuple | x5 | Red | 16 | 5 bullets per shot | One-time |

### Power-Up Drop Logic
- Only drop from Bonus Ships
- Weighted random selection
- Filters out maxed/owned upgrades
- Higher weights for useful upgrades

---

## Alien Types (All 24 Types)

| Level | Name | Health | Speed | Points | Size | Shape | Color | Flight Pattern |
|-------|------|--------|-------|--------|------|-------|-------|----------------|
| 1 | Yellow Invader | 1 | 1.1 | 10 | 10 | invader1 | Yellow/Orange | Straight wobble |
| 2 | Blue Zapper | 1 | 1.2 | 12 | 9 | invader2 | Blue | Straight wobble |
| 2 | **Rainbow Bonus** | 1 | 1.7 | 100 | 12 | bonus | Cyan | Straight wobble |
| 3 | Green Scout | 1 | 1.3 | 15 | 8 | invader2 | Green | Straight wobble |
| 4 | Pink Weaver | 1 | 1.24 | 14 | 9 | invader1 | Pink | Straight wobble |
| 5 | Orange UFO | 2 | 1.1 | 30 | 12 | invader3 | Orange | Straight wobble |
| 6 | Cyan Dasher | 1 | 1.5 | 16 | 9 | invader1 | Cyan | Straight wobble |
| 7 | Red Dart | 1 | 2.0 | 20 | 8 | invader1 | Red | Kamikaze charge |
| 8 | Purple Bomber | 2 | 1.3 | 35 | 14 | invader2 | Purple | Straight wobble |
| 9 | Lime Runner | 1 | 1.6 | 18 | 9 | invader2 | Lime | Straight wobble |
| 10 | Cyan Spinner | 1 | 1.0 | 18 | 10 | invader1 | Cyan | Spiral inward |
| 11 | Red Battleship | 3 | 0.9 | 60 | 18 | invader3 | Red | Slow drift |
| 12 | Teal Striker | 2 | 1.8 | 28 | 10 | invader1 | Teal | Straight wobble |
| 13 | White Speeder | 1 | 2.4 | 25 | 9 | invader1 | White | Spiral inward |
| 14 | Blue Tank | 3 | 1.06 | 50 | 16 | invader3 | Blue | Slow drift |
| 15 | Purple Cruiser | 3 | 1.2 | 55 | 15 | invader2 | Purple | Slow drift |
| 16 | Magenta Zigzag | 1 | 1.5 | 22 | 10 | invader1 | Magenta | Sine wave zigzag |
| 17 | Green Heavy | 3 | 0.9 | 58 | 17 | invader3 | Green | Slow drift |
| 18 | Orange Kamikaze | 1 | 2.6 | 30 | 8 | invader1 | Orange | Kamikaze charge |
| 19 | Cyan Destroyer | 4 | 1.1 | 80 | 18 | invader3 | Cyan | Slow drift |
| 19 | Yellow Interceptor | 2 | 1.9 | 40 | 11 | invader2 | Yellow | S-wave |
| 20 | Pink Sparkler | 1 | 1.4 | 20 | 10 | invader1 | Pink | Straight wobble |
| 20 | Orange Dreadnought | 4 | 1.0 | 90 | 20 | invader3 | Orange | Slow drift |
| 20 | White Phantom | 2 | 2.0 | 45 | 11 | invader2 | White | Teleport jumps |
| 20 | Black Boss | 5 | 1.2 | 150 | 22 | invader3 | Black | Slow drift |

---

## Flight Patterns

### 1. Straight Wobble (Default)
- Move directly toward Earth
- Slight perpendicular wobble (sine wave)
- Used by most basic aliens

### 2. Kamikaze Charge
- Straight aggressive charge at 1.2x speed
- No wobble
- Used by: Red Dart (L7), Orange Kamikaze (L18)

### 3. Spiral Inward
- Orbit around Earth while moving closer
- Gradual spiral trajectory
- Used by: Cyan Spinner (L10), White Speeder (L13)

### 4. Slow Drift
- Move toward Earth with periodic course corrections
- Recalculate direction every 2 seconds
- 0.7x speed
- Used by: Heavy/Tank/Battleship enemies (L11+)

### 5. Sine Wave Zigzag
- Move toward Earth with perpendicular zigzag
- Moderate amplitude sine wave
- Used by: Magenta Zigzag (L16)

### 6. S-Wave
- Subtle S-shaped approach pattern
- Similar to zigzag but smoother
- Used by: Yellow Interceptor (L19)

### 7. Teleport Jumps
- Jump forward 60 pixels every 1.5 seconds
- Slow drift between jumps (0.3x speed)
- Used by: White Phantom (L20)

---

## Wave Progression

### Wave Targets (Aliens to Kill)

| Wave Range | Formula | Example Values |
|------------|---------|----------------|
| L1-5 | 5 + wave × 3 | L1=8, L2=11, L3=14, L4=17, L5=20 |
| L6-10 | 10 + wave × 4 | L6=34, L7=38, L8=42, L9=46, L10=50 |
| L11-15 | 15 + wave × 4 | L11=59, L12=63, L13=67, L14=71, L15=75 |
| L16-20 | 20 + wave × 4 | L16=84, L17=88, L18=92, L19=96, L20=100 |

### Spawn Rate (Frames Between Spawns)

| Wave Range | Formula | Speed Increase |
|------------|---------|----------------|
| L1-5 | 165 - wave × 15 | L1=150, L5=90 |
| L6-10 | 135 - wave × 9 | L6=81, L10=45 |
| L11-15 | 90 - wave × 5 | L11=35, L15=15 |
| L16-20 | 55 - wave | L16=39, L20=35 |
| Minimum | 20 frames | Never faster than 20 |

**Result**: Enemies spawn progressively faster as waves advance.

### Speed Multiplier (Applied to All Aliens)

| Wave Range | Formula | Example Values |
|------------|---------|----------------|
| L1-5 | 1 + (wave - 1) × 0.1 | L1=1.0x, L5=1.4x |
| L6-10 | 1.4 + (wave - 5) × 0.12 | L6=1.52x, L10=2.0x |
| L11-15 | 2.0 + (wave - 10) × 0.16 | L11=2.16x, L15=2.8x |
| L16-20 | 2.8 + (wave - 15) × 0.24 | L16=3.04x, L20=4.0x |

**Result**: All aliens move 4x faster at L20 compared to L1.

### Bonus Ship Spawns

Bonus ships spawn based on kill count:

| Wave Range | Kills Per Bonus |
|------------|-----------------|
| L1 | None |
| L2-5 | Every 100 kills |
| L6-10 | Every 60 kills |
| L11-15 | Every 40 kills |
| L16-20 | Every 30 kills |

---

## Alien Spawn Weights

Weighted random selection determines which alien types spawn:

### Base Weights
- Yellow Invader: 40 (L1-5), 25 (L6-10), 20 (L11-15), 15 (L16-20)
- Bonus Ships: 5
- Fast Singles (health=1, speed≥0.7): 15
- Heavy Multi-hit (health≥3): 10 (L1-14), 15 (L15-20)
- Orange Kamikaze: 5 (L1-13), 10 (L14-20)

**Result**: Common enemies dominate early, tougher enemies increase later.

---

## Level Breakdown (Summary)

| Level | Aliens to Kill | New Alien Type(s) | New Power-Up(s) | Special Feature |
|-------|----------------|-------------------|-----------------|-----------------|
| 1 | 8 | Yellow Invader | Speed, x2, FR, +HP | Tutorial level |
| 2 | 11 | Blue Zapper, **Bonus Ship** | - | Power-ups begin |
| 3 | 14 | Green Scout | - | - |
| 4 | 17 | Pink Weaver | - | - |
| 5 | 20 | Orange UFO (2HP) | - | First 2-health enemy |
| 6 | 34 | Cyan Dasher | x3 (Triple Shot) | Major difficulty spike |
| 7 | 38 | Red Dart (Kamikaze) | MSL (Missiles) | First special movement |
| 8 | 42 | Purple Bomber (2HP) | - | - |
| 9 | 46 | Lime Runner | - | - |
| 10 | 50 | Cyan Spinner (Spiral) | - | Spiral movement |
| 11 | 59 | Red Battleship (3HP) | 1UP (Extra Life) | First 3-health enemy |
| 12 | 63 | Teal Striker (2HP) | x4, 2X (Mirror) | Multiple new powers |
| 13 | 67 | White Speeder (Spiral) | BOOM (Screen Clear) | Emergency power |
| 14 | 71 | Blue Tank (3HP) | - | - |
| 15 | 75 | Purple Cruiser (3HP) | - | - |
| 16 | 84 | Magenta Zigzag | x5 (Quintuple) | Zigzag pattern |
| 17 | 88 | Green Heavy (3HP) | - | - |
| 18 | 92 | Orange Kamikaze | - | Fast kamikaze |
| 19 | 96 | Cyan Destroyer (4HP), Yellow Interceptor (S-wave) | - | First 4-health, S-wave |
| 20 | 100 | 4 new types including Black Boss (5HP) | - | Final boss wave |

---

## Scoring System

### Points by Alien Type
- 1 HP aliens: 10-30 points
- 2 HP aliens: 12-45 points
- 3 HP aliens: 50-60 points
- 4 HP aliens: 80-90 points
- 5 HP aliens: 150 points (Black Boss)
- Bonus Ships: 100 points

### Extra Lives
- Awarded every 10,000 points
- Maximum 5 lives total

---

## Death Penalty System

When player loses a life:

### Speed Loss
- Lose 1 upgrade OR 25% (whichever is MORE)
- Never goes below starting speed (0.035)

### Fire Rate Loss
- Lose 1 upgrade OR 25% (whichever is MORE)
- Never goes below starting rate (30 frames)

### Weapon Downgrade
- Lose 1 bullet tier (5→4→3→2→1)
- Missiles: 50% chance to lose

### Power Loss
- Auto-fire: ALWAYS lost
- Mirror ship: Cleared if active
- Screen clear: Cleared if held

**Philosophy**: Hybrid penalty that's meaningful but not devastating.

---

## Audio System

### Sound Effects (8-bit Procedural)
- Shooting (square wave)
- Small/Medium/Large explosions (different complexity)
- Bonus ship sparkle
- Power-up collection shimmer
- Extra life fanfare
- Screen clear massive explosion
- Ship destruction (dramatic)
- Earth impact crash
- Level complete victory fanfare

### Background Music
- 5 different 32-bar classical-inspired tracks
- Rotate through tracks with each wave
- 8-bit chiptune style
- Square wave melody, triangle harmony
- Can be toggled on/off (DEL key)

---

## Visual Design

### Aesthetic
- Retro green-on-black terminal (Matrix-like)
- 8-bit pixel art
- Glowing effects
- Particle explosions

### Ship Visual Changes
- Green: Single shot (default)
- Purple: Double shot
- Blue/Cyan: Triple shot
- Red/Orange: Quintuple shot
- Ship grows with power-ups
- Auto-fire adds pulsing glow
- Speed adds trailing effects

### Earth
- Rotating continents (pixel art)
- Blue oceans
- Green/brown landmasses
- White clouds (static)
- Atmospheric glow

---

## UI Elements

### HUD
- Large score display (top-left)
- 7 status bars (left sidebar)
- High score leaderboard (top-right)
- Controls guide (right side)
- Sound toggle

### Game States
- Start screen ("Press P to Play")
- Playing
- Paused ("Press ENTER to resume")
- Wave complete message
- Game over screen
- Victory screen (after L20)

---

## Persistence

### localStorage
- Key: `spaceDefenseHighScores`
- Stores top 5 scores
- Data: `{ score, wave, name, date }`
- Displayed in real-time leaderboard

---

## Technical Specifications

### Performance
- Target: 60 FPS
- Single HTML file: ~140KB
- No external dependencies
- Runs entirely client-side

### Browser Requirements
- HTML5 Canvas
- Web Audio API
- localStorage
- Modern JavaScript (ES6+)

---

## Known Issues / Limitations

1. **Bullet speed never increases** - Fixed at 12.0
2. **No difficulty options** - Single difficulty curve
3. **No pause during explosions** - Can feel unfair
4. **Bonus ship spawns can cluster** - RNG dependent
5. **Wave 20 has no unique mechanics** - Just harder enemies

---

## Game Balance Notes

### Early Game (L1-5)
- Forgiving spawn rates
- Simple straight-line enemies
- Plentiful power-ups
- Learn mechanics

### Mid Game (L6-15)
- Difficulty ramps significantly at L6
- New movement patterns introduced
- Multi-hit enemies become common
- Strategic power-up management required

### Late Game (L16-20)
- 4x speed multiplier
- Complex movement patterns
- High-health enemies
- Screen clear becomes essential
- Precision and upgrades critical

---

## Victory Condition

Complete Wave 20 (100 kills) to win the game.
Currently no content after Wave 20 - game simply displays victory.

---

# Proposed Changes

## Priority 1: Add Bullet Speed Metric

### Current Issue
Bullet speed is fixed at 12.0 pixels/frame and never changes throughout the game. This removes a key progression mechanic.

### Proposed Solution
Add bullet speed as an upgradeable stat with power-up and status bar.

### Implementation Specification

#### Game State Changes
- [x] Add `bulletSpeed` to player object in gameState
- [x] Set initial value to 5.0 (instead of fixed 12.0)
- [x] Add cap at 12.0

#### Power-Up System
- [x] Add new power-up type to `powerUpTypes` array:
  - **Type**: `bulletspeed`
  - **Label**: `BLT`
  - **Color**: `#00ddff` (bright cyan)
  - **minLevel**: 2
- [x] Add collection logic: +0.5 per pickup
- [x] Add weight logic to filter out when bulletSpeed >= 12.0

#### UI Changes
- [x] Add 8th status bar after SUPERPOWERS
- [x] Label: "BULLET SPD"
- [x] Color: Bright Cyan (#00ddff)
- [x] Calculation: `(bulletSpeed - 5.0) / 7.0 × 100%`

#### Code Updates
- [x] Replace all hardcoded `bulletSpeed = 12.0` with `gameState.player.bulletSpeed`
- [x] Update `shoot()` function to use dynamic bullet speed
- [x] Update `shootMirrorShip()` function to use dynamic bullet speed

#### Death Penalty
- [x] Add bullet speed to death penalty system
- [x] Lose 1 upgrade OR 25% (whichever is MORE)
- [x] Never goes below starting speed (5.0)

#### Testing Checklist
- [x] Bullets start very slow at game start
- [x] Bullet speed increases with power-up collection
- [x] Status bar updates correctly
- [x] Cap at 12.0 works (no further upgrades)
- [x] Death penalty reduces bullet speed appropriately
- [x] Power-up stops dropping when maxed

**Status**: ✅ COMPLETED - Implemented 2025-12-11

### Progression Summary
- **Start**: 5.0 pixels/frame
- **Max**: 12.0 pixels/frame
- **Increment**: +0.5 per power-up
- **Total upgrades**: 14 (to reach max from min)
- **Unlock Level**: 2


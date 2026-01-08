# Space Defense - Project Documentation

## Project Overview

**Space Defense** is a retro-style 8-bit browser-based arcade game where players defend Earth from alien invasions by orbiting around the planet and shooting incoming enemies.

## Project Type

- **Type**: Static HTML Game
- **Technology**: Pure HTML5 Canvas + Vanilla JavaScript
- **Architecture**: Single-file application (index.html)
- **No Dependencies**: No frameworks, libraries, or build tools required

## How to Run

**Recommended Method (for full functionality):**
```bash
# Navigate to project directory
cd /Users/mat.edwards/dev/test-claude/space-web/

# Start local web server on port 3110
python3 -m http.server 3110

# Open in browser
open http://localhost:3110/index.html
```

**Note**: The game requires a web server to load `game-config.json`. Opening `index.html` directly via `file://` protocol will fail due to CORS restrictions.

**Port Assignment**: 3110 (documented in root CLAUDE.md)

## Game Mechanics

### Core Gameplay
- Player ship orbits around Earth at fixed radius
- Aliens spawn from space and move toward Earth
- Shoot aliens before they reach Earth
- Survive waves of increasing difficulty
- Collect power-ups from bonus ships

### Controls
- **Q**: Rotate counter-clockwise
- **A**: Rotate clockwise
- **P**: Fire weapon (hold for auto-fire when upgraded)
- **O**: Launch missile (when available)
- **B**: Screen clear bomb (when available)
- **ENTER**: Pause/Resume
- **DEL**: Toggle sound on/off

### Enemy Types
1. **Small Aliens** (Yellow/Orange) - Fast, low health, 10 points
2. **Medium UFOs** (Orange/Red) - Moderate speed, 25 points
3. **Large Boss Ships** (Red/Dark Red) - Slow, high health, 50 points
4. **Bonus Ships** (Cyan) - Rare, drop power-ups, 100 points

### Power-Up System

| Power-Up | Effect | Description |
|----------|--------|-------------|
| Speed | +Movement | Faster orbit rotation |
| Firepower | x2, x3, x5 | Multiple simultaneous shots |
| Shot Speed | +Velocity | Bullets travel faster |
| Missiles | Special weapon | Homing missiles (O key) |
| Auto-Fire | Continuous | Hold P for rapid fire |
| Mirror Ship | Dual control | Second ship on opposite side |
| Screen Clear | Bomb | Destroy all enemies (B key) |
| Extra Life | +1 Life | Additional life |
| Earth HP | +Health | Restore Earth's health |

### Med Ship System

A special ship that appears during gameplay to provide health support:

- **Spawn timing**: Waves 3, 5, 7, 9 (every other), then every wave from 10+
- **Appears**: After 5 kills on eligible waves
- **Two phases**:
  1. **Alien phase**: Red/orange hostile ship - shoot to activate
  2. **Med Ship phase**: White/silver ship with red cross - each hit drops a med pack
- **Med packs**: White cases with red cross that heal Earth +1 HP when they reach it
- **Music**: Pulsing sci-fi theme plays while med ship is on screen

### Superweapon Pack System

Superweapons spawn as a single bright pickup with a number:

- **Appearance**: Rainbow-cycling white circle with number 1-5 inside
- **Effect**: Collecting gives that many random superweapons (e.g., "3" = 3 random superweapons)
- **Cooldown**: 1-second delay after collection before superweapons can be triggered (prevents accidental use)
- **Auto-collect**: Superweapons and lives auto-collect when reaching Earth

### Superweapons

| Key | Superweapon | Effect |
|-----|-------------|--------|
| 1 | Shield | Pulsing ring around Earth that kills aliens |
| 2 | HyperKill | Instantly destroys all enemies on screen |
| 3 | Satellites | 4 satellites in formation that rotate and shoot |
| 4 | Quad | 4 ships around Earth (same firepower as main ship) |
| 5 | Rockets | All bullets become homing mini rockets |

### Progression System
- Waves increase in difficulty
- More enemies spawn each wave
- Enemy speed increases
- Health/damage scales up
- Score multipliers increase
- Background music rotates through 5 classical themes

## Technical Architecture

### File Structure
```
space-web/
├── index.html          (~4,200 lines - complete game)
├── game-config.json   (773 lines - game balance & mechanics)
└── CLAUDE.md          (this file)
```

### Configuration-Driven Architecture

**game-config.json** controls all game mechanics and balance:

- **alienTypes[]**: 24 alien definitions with health, speed, points, colors, and flightPattern assignment
- **powerUpTypes[]**: 12 power-up definitions with unlock levels and effects
- **levels[]**: 20 wave configurations with spawn rates, speed multipliers, and difficulty scaling
- **flightPatterns{}**: Movement pattern definitions with tunable parameters
  - `wobbleAmplitude`, `wobbleFrequency` for zigzag/straight_wobble/swave patterns
  - `orbitSpeed`, `inwardSpeedMultiplier` for spiral pattern
  - `jumpInterval`, `jumpDistance`, `driftSpeedMultiplier` for teleport pattern
  - `speedMultiplier` for kamikaze pattern
  - `recalculateInterval`, `speedMultiplier` for slow_drift pattern
- **player{}**: Starting stats (lives, health, speeds, fire rate)
- **deathPenalty{}**: Penalty system configuration

**Benefits:**
- Adjust all game balance without touching code
- Tune alien movement by changing flightPattern parameters in JSON
- Modify wave difficulty by editing level configs
- Easy to experiment with different balance configurations

### Code Organization (within index.html)

1. **CSS Styling** (lines 8-327)
   - Retro terminal aesthetic (green-on-black)
   - UI overlays (score, leaderboard, status bars)
   - Game states (start, pause, game over)
   - Animations and effects

2. **Game State Management**
   - Player object (position, speed, weapons)
   - Aliens array (enemies and bonuses)
   - Bullets and missiles
   - Power-ups and collectibles
   - Wave progression

3. **Audio System** (Web Audio API)
   - Procedurally generated 8-bit sounds
   - Shooting, explosions, power-ups
   - 5 rotating background music tracks
   - Sound toggle functionality

4. **Rendering System**
   - Canvas-based pixel art
   - Earth with rotating continents
   - 8-bit alien designs
   - Particle effects for explosions
   - Visual power-up indicators

5. **High Score System**
   - localStorage persistence
   - Top 5 scores tracking
   - Wave progression recording
   - Real-time leaderboard display

### Key Functions

| Function | Purpose |
|----------|---------|
| `drawEarth()` | Renders Earth with rotating continents |
| `draw8BitAlien()` | Draws different alien types |
| `drawPlayer()` | Renders player ship with power-up visuals |
| `updatePlayer()` | Handles player movement and shooting |
| `spawnAliens()` | Wave-based enemy spawning logic |
| `shoot()` | Creates bullets based on firepower level |
| `dropPowerUp()` | Random power-up generation |
| `playMusicLoop()` | Rotates through background music tracks |

## Game Balance

### Lives & Health
- Starting Lives: 3
- Earth Health: 10 hits
- Losing conditions: Lives = 0 OR Earth Health = 0

### Scoring
- Small Alien: 10 points
- Medium UFO: 25 points
- Large Boss: 50 points
- Bonus Ship: 100 points
- Extra Life awarded every 10,000 points

### Wave Scaling
- Enemy count increases each wave
- Movement speed increases gradually
- Health values scale up
- More dangerous enemy types appear

## Browser Compatibility

- **Chrome/Edge**: Full support
- **Firefox**: Full support
- **Safari**: Full support
- **Requirements**:
  - HTML5 Canvas support
  - Web Audio API support
  - localStorage support
  - Modern JavaScript (ES6+)

## Performance

- Single HTML file: ~140KB
- No external dependencies
- No network requests (after initial load)
- Runs entirely client-side
- 60 FPS target frame rate

## Data Persistence

High scores stored in localStorage:
- Key: `spaceDefenseHighScores`
- Format: JSON array of top 5 scores
- Data: `{ score, wave, name, date }`

## Known Features

1. Ship grows visually with power-ups
2. Color schemes change based on weapon level:
   - Green: Single shot (default)
   - Purple: Double shot
   - Blue/Cyan: Triple shot
   - Red/Orange: Quintuple shot
3. Auto-fire creates pulsing glow effect
4. Speed trails appear at high speed levels
5. Music track changes with each wave

## Git Repository

- **Status**: Not yet linked to GitHub repository
- **Recommended**: `mat-e-exp/space-web.git`

## Development Notes

### Adding New Features
All game logic is in single HTML file. Key areas to modify:

- **New alien types**: Update `alienTypes` object and `draw8BitAlien()` function
- **New power-ups**: Add to power-up spawn logic and collection handling
- **Audio changes**: Modify sound generation functions (8-bit synthesis)
- **Balance tweaks**: Adjust constants in game state initialization

### Testing
- Open index.html in browser
- Use browser console for debugging
- Check localStorage for high score data
- Test sound with ON/OFF toggle

## Status

- **Current State**: Fully functional game
- **Last Modified**: 2025-12-11
- **Lines of Code**: 3,442
- **Completeness**: Production ready

## Future Considerations

Potential enhancements (not currently implemented):
- Additional alien types or boss battles
- More power-up varieties
- Achievement system
- Multiplayer mode
- Mobile touch controls
- Save/load game state

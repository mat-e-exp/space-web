# Space Defense

A retro-style 8-bit browser-based arcade game where you defend Earth from alien invasions by orbiting around the planet and shooting incoming enemies.

![Space Defense](https://img.shields.io/badge/Game-HTML5%20Canvas-green) ![No Dependencies](https://img.shields.io/badge/Dependencies-None-blue)

## Play Now

**[Play on GitHub Pages](https://mat-e-exp.github.io/space-web/)** (once deployed)

Or run locally:
```bash
python3 -m http.server 3110
open http://localhost:3110/index.html
```

## Controls

| Key | Action |
|-----|--------|
| **Q** | Rotate counter-clockwise |
| **A** | Rotate clockwise |
| **P** | Fire weapon (hold for auto-fire when upgraded) |
| **O** | Launch missile |
| **B** | Screen clear bomb |
| **1-5** | Activate superweapons |
| **ENTER** | Pause/Resume |
| **DEL** | Toggle sound |
| **C** | Configuration (start screen) |
| **=** | Level select (start screen) |

## Features

- 20 waves of increasing difficulty
- 24 unique alien types with different flight patterns
- 12 power-up types (speed, firepower, missiles, mirror ship, etc.)
- 5 superweapons (shield, hyperkill, satellites, quad ship, rockets)
- Procedurally generated 8-bit music and sound effects
- Victory celebration with fireworks and credits
- High score leaderboard (localStorage)
- In-game configuration with difficulty presets

## Enemy Types

| Type | Points | Description |
|------|--------|-------------|
| Small Aliens | 10 | Fast, low health |
| Medium UFOs | 25 | Moderate speed |
| Large Boss Ships | 50 | Slow, high health |
| Bonus Ships | 100 | Drop power-ups |

## Superweapons

Unlock at wave 5. Collect multiple of each type.

| # | Weapon | Effect |
|---|--------|--------|
| 1 | Shield | Protective barrier around Earth |
| 2 | Hyperkill | Destroy all enemies on screen |
| 3 | Satellites | Orbiting guns that auto-fire |
| 4 | Quad Ship | 4 ships at cardinal points |
| 5 | Rockets | Homing missiles barrage |

## Technical Details

- Pure HTML5 Canvas + Vanilla JavaScript
- No frameworks or dependencies
- Single-file application (~6000 lines)
- Web Audio API for procedural audio
- 60 FPS target frame rate
- Configuration-driven game balance via `game-config.json`

## Configuration

Press **C** on the start screen to access the configuration panel. Adjust:
- Player settings (lives, speed, fire rate)
- Combat mechanics (damage, missile behavior)
- Enemy difficulty (speed, health, spawn rates)
- Power-up frequency and duration
- Superweapon parameters

Presets available: Easy, Normal, Hard, Insane

## Browser Support

- Chrome/Edge: Full support
- Firefox: Full support
- Safari: Full support

Requires: HTML5 Canvas, Web Audio API, localStorage, ES6+

## License

MIT

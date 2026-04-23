# ChargeState

> A 2D top-down physics puzzle game built in Unity where polarity is your only weapon.

**Team of 4 · Unity 2D · C# · 3 Levels**

---

<img width="1512" height="982" alt="ChargeState-1" src="https://github.com/user-attachments/assets/1178a16b-7b45-4c66-866f-6666d85131a2" />


---

## Overview

In ChargeState, you control a magnetic entity that can switch between **red** and **blue** polarity. The goal is to collect all coins on each level — but coins and missiles share the same polarity system. Opposite charges attract; same charges repel. Every switch changes your relationship to every object on screen at once.

<img width="1512" height="982" alt="ChargeState-2" src="https://github.com/user-attachments/assets/c35610cd-34f9-4807-9a77-c27bd40a5acf" />


Missiles fired by turrets track you using a custom physics model, and their behavior changes based on whether they share your current charge. Survive the missiles and collect all coins to advance.

---

## Core Mechanics

### Polarity Switching
The player toggles between red and blue at will. This single input controls everything:
- **Opposite-color coins** are pulled toward the player (collect them)
- **Same-color coins** are pushed away (reposition before switching)
- **Opposite-color missiles** are pulled in — dangerous if mistimed
- **Same-color missiles** are pushed away — use this to deflect them

### Van der Waals–Inspired Force Model
Missile attraction and repulsion use a custom algorithm modeled after van der Waals force curves rather than simple linear attraction. Key properties:

- **Distance-dependent force magnitude** — force peaks at an intermediate range and falls off at both extremes
- **Minimum-distance clamping** — prevents degenerate close-range behavior and enables "slingshot" redirects where a missile swings around the player and launches away
- **Directional blending** — force vectors are blended with the missile's existing velocity to produce curved, organic pursuit paths rather than mechanical straight-line tracking

This produces missiles that feel alive: they arc, curve, and occasionally loop before correcting course.

### Missile Behavior
Each missile has multiple behavioral states driven by `MissileScript.cs`:
- **Curved pursuit steering** — continuously recalculates heading toward the player using the force model above
- **Telegraphed detonation** — a visible wind-up (color flash) signals imminent explosion, giving the player a reaction window
- **AoE destructible damage** — the explosion radius can destroy other objects in the scene, enabling chain reactions

### Turrets
`MissileTurretScript.cs` handles firing patterns including burst ("gatling") mode, managing cooldowns, missile instantiation, and charge assignment per shot.

### Power-Ups
`NeutralizePowerUp.cs` provides a temporary neutral state, making the player immune to both attraction and repulsion — useful for navigating dense coin layouts without triggering missiles.

---

## Codebase Overview

| Script | Responsibility |
|---|---|
| `PlayerScript.cs` | Player movement, polarity switching, collision handling |
| `MissileScript.cs` | Van der Waals force model, curved pursuit, timed detonation, AoE |
| `MissileTurretScript.cs` | Firing patterns, burst/gatling mode, missile instantiation |
| `CoinScript.cs` | Coin polarity state, attraction/repulsion response, collection logic |
| `EnemyScript.cs` | Enemy entity behavior and polarity interactions |
| `NeutralizePowerUp.cs` | Temporary charge-neutral power-up logic |
| `GameManager.cs` | Scene flow, level progression, game state |
| `CameraScript.cs` | Camera follow + screen shake on player death |
| `AudioManager.cs` | SFX management and audio event dispatching |
| `BackgroundMusicController.cs` | Per-level music, auto level-change triggers |
| `PauseMenu.cs` | Pause/resume UI, menu transitions |
| `KillAfterTime.cs` | Despawn utility for projectiles and effects |
| `ResetAfterTime.cs` | Timed object reset utility |
| `MainRestart.cs` | Main menu restart handler |
| `AmariPlayerScript.cs` / `AmariCoinScript.cs` | Early microprototype scripts (retained for reference) |

<img width="640" height="320" alt="ChargeState-MagnetGIF" src="https://github.com/user-attachments/assets/1193eba0-2a53-41db-8329-183fa403fa24" />

---

## Controls

| Input | Action |
|---|---|
| `WASD` / Arrow Keys | Move |
| `Space` | Switch polarity |
| `Escape` | Pause |

---

## Levels

The game ships with 3 levels of increasing complexity, introducing new turret placements, tighter coin layouts, and scenarios designed to force the player to use polarity switching offensively rather than just defensively.

---

## Tech

- **Engine:** Unity 2022 (2D)
- **Language:** C#
- **Physics:** Unity Physics2D with custom force calculations
- **Rendering:** ShaderLab / HLSL custom shaders for polarity visual effects
- **Team Size:** 4

---

## Team

Built by a team of 4 for CSCI 426 — Game Prototyping at the University of Southern California.

- Tony Olivares (https://github.com/tonyolives)
- Raachel Brockman (https://www.linkedin.com/in/rachel-brockman-game-designer/)
- Amari McClendon (https://www.amarimcclendon.info/)
- Andy Park (https://www.linkedin.com/in/andy-park-b5a76a236/)

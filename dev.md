# Snake Game - Development Documentation

## Project Overview

A modern-style Snake game built with HTML5 Canvas and vanilla JavaScript, using a responsive design that supports both desktop and mobile interaction.

---

## Tech Stack

| Technology | Description |
|------|------|
| HTML5 Canvas | Game rendering |
| JavaScript (ES6+) | Game logic control |
| Tailwind CSS | UI styling and layout |
| Google Fonts | Modern typography |

---

## Core Features

### 1. Basic Gameplay

#### 1.1 Snake Controls
- **Control methods**
  - Keyboard: arrow keys (↑↓←→) or WASD
  - Touchscreen: virtual directional buttons are shown on mobile devices

- **Movement mechanics**
  - The snake head always moves in the current direction
  - Reverse movement is prevented, for example, moving left immediately after moving right
  - `nextDirection` is used as an input buffer and applied once per frame to avoid self-collision bugs caused by rapid key presses

#### 1.2 Food System
- Food spawns at a random position and is guaranteed not to overlap with the snake
- When food is eaten:
  - Score +10
  - The snake grows longer by not removing the tail
  - A particle effect is triggered
  - Movement speed increases

#### 1.3 Collision Detection
- **Wall collision**: the snake head touches the edge of the canvas
- **Self collision**: the snake head overlaps with any segment of its own body

#### 1.4 Speed System
- Initial speed: 150ms/frame
- Speed increases by 2ms each time food is eaten
- Minimum speed: 50ms (fastest)

---

### 2. Scoring System

#### 2.1 Score Types
| Type | Description | Storage |
|------|------|----------|
| Current Score | Score for the current session | In-memory variable `state.score` |
| High Score | Best historical result | Persisted in `localStorage` |

#### 2.2 Score Calculation
- Each food item eaten: `+10 points`
- The high score is automatically saved to browser local storage
- Storage key: `snakeHighScore`

---

### 3. Game State Management

#### 3.1 State Flow

```
┌─────────────────────────────────────────────────────────┐
│                    Initial Load                        │
│   (Show the "Start Game" overlay and load high score)  │
└──────────────────┬──────────────────────────────────────┘
                   │
                   ▼
        ┌──────────────────┐
        │   Click "Start Game" │
        └────────┬─────────┘
                 │
                 ▼
    ┌────────────────────────┐
    │      Game Running      │
    │  - Snake keeps moving  │
    │  - Render in real time │
    │  - Detect collisions / │
    │    food consumption    │
    └────────┬───────────────┘
             │
     ┌───────┴───────┐
     │               │
     ▼               ▼
┌──────────┐  ┌──────────────┐
│ Eat Food │  │ Collision /  │
│ +10 pts  │  │ Hit Wall     │
│ Speed Up │  │ (Game Over)  │
│ Spawn New│  └──────┬───────┘
│ Food     │         │
└──────────┘         │
     │                ▼
     └──────────┐ ┌──────────────┐
                │ │ Show Game Over│
                │ │ Update High   │
                │ │ Score         │
                └─►│ Click Replay  │
                   │ Reset Game    │
                   └──────────────┘
```

#### 3.2 State Data Structure

```javascript
{
    snake: [                     // Snake body array (from head to tail)
        { x: 400, y: 300 },     // Head
        { x: 380, y: 300 },     // Body
        { x: 360, y: 300 }      // Tail
    ],
    food: { x: 200, y: 150 },   // Food position
    direction: 'right',         // Current direction
    nextDirection: 'right',     // Direction for the next frame (input buffer)
    score: 0,                   // Current score
    highScore: 150,             // High score (loaded from localStorage)
    gameLoop: null,             // setInterval timer ID
    isPlaying: false,           // Whether the game is running
    isPaused: false,            // Whether the game is paused
    speed: 150                  // Current speed (ms)
}
```

---

### 4. Persistent Storage

#### 4.1 Storage Location
```
localStorage
└── snakeHighScore: "150"  // String value storing the high score
```

#### 4.2 When Data Is Saved
- **On game over**: automatically updates if the current score exceeds the high score
- **On page load**: reads and displays the high score

#### 4.3 Data Structure
| Key | Value Type | Description |
|------|--------|------|
| `snakeHighScore` | String | Stores the high score as an integer string |

---

### 5. Visual Effects

#### 5.1 Animation Effects
| Effect | Implementation | CSS Animation |
|------|----------|----------|
| Snake Head Breathing | Scale animation | `headBreathe` 2s infinite |
| Food Pulse | Scale + outer glow | `foodPulse` 1.5s infinite |
| Particle Explosion | JS-generated + CSS animation | `particleFade` 0.8s |
| Floating Score | Text floats upward | `scoreFloatUp` 0.8s |
| Overlay Modal | Scale + opacity | CSS transition |

#### 5.2 Effect Triggers
- **Eating food**: 8 particles spread outward + a floating `+10` text
- **Death**: the snake head explodes into 20 fragments

---

## Key Function Notes

### Core Game Functions

| Function | Purpose | Trigger Timing |
|------|------|----------|
| `resetGame()` | Initialize the snake, score, and speed | On start / restart |
| `spawnFood()` | Spawn random food without overlap | After each food pickup |
| `gameLoop()` | Main loop: movement + collision detection | Called by timer |
| `checkCollision()` | Detect wall hits or self-collision | After each movement frame |
| `draw()` | Clear the canvas and redraw all content | Every frame |
| `drawSnake()` | Render the snake body with gradients | Every frame |
| `drawFood()` | Render the glowing food | Every frame |

### Helper Functions

| Function | Purpose |
|------|------|
| `isOnSnake()` | Check whether a coordinate is on the snake |
| `increaseSpeed()` | Increase speed after eating food |
| `roundedRect()` | Draw a rounded rectangle |
| `createFoodEffect()` | Particle effect when food is eaten |
| `createDeathEffect()` | Fragment effect on death |

---

## File Structure

```
llm-base/
├── index.html          # Main game file (HTML + CSS + JS in one file)
└── DEVELOPMENT.md      # This development document
```

---

## Extension Suggestions

### Features That Could Be Added
1. **Pause feature**: press the space bar to pause/resume
2. **Difficulty selection**: provide easy/normal/hard modes on the start screen
3. **Sound effects**: sounds for eating food and dying
4. **Skin system**: customizable visual styles for the snake and food
5. **Leaderboard**: display records from the last 10 games
6. **Improved save system**: record game history, unlocked achievements, and more

### Code Optimization Suggestions
1. Split CSS and JS into separate files
2. Use ES Modules for modularization
3. Add TypeScript type definitions
4. Introduce a game framework such as Phaser or Kaboom.js

---

## Browser Compatibility

- Modern versions of Chrome / Edge / Safari / Firefox
- Mobile: iOS Safari, Android Chrome
- Requires support for Canvas and ES6+ syntax

---

## Data Source Notes

| Data Source | Description | Persisted |
|----------|------|--------|
| `highScore` | Read from `localStorage.snakeHighScore` | Yes |
| Snake data | Calculated dynamically during gameplay | No |
| Food position | Randomly generated during gameplay | No |
| Score | Accumulated during gameplay | No (high score only) |
| Speed | Adjusted dynamically based on score | No |

---

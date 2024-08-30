# Emoji Matching Game

Welcome to the Emoji Matching Game! This project is a fun and engaging game where players match pairs of identical emojis within a time limit. The game is built using SvelteKit and features a responsive design, intuitive gameplay, and customizable options.

## Table of Contents

- [Features](#features)
- [Installation](#installation)
- [Usage](#usage)
- [Game Rules](#game-rules)
- [Customization](#customization)
- [Development](#development)
- [Contributing](#contributing)
- [License](#license)

## Features

- **Interactive Gameplay**: Flip cards to reveal emojis and match pairs within a time limit.
- **Responsive Design**: The game is fully responsive, working seamlessly on both desktop and mobile devices.
- **Timer**: A countdown timer adds urgency to the game, challenging players to match all pairs before time runs out.
- **Pause Feature**: Players can pause the game using the Escape key and resume when ready.
- **Dynamic Animations**: Smooth and engaging animations enhance the user experience.
- **Winning and Losing States**: The game provides clear feedback when the player wins or loses.

## Installation

To run the game locally, follow these steps:

1. **Clone the repository:**

   ```
   git clone https://github.com/n01syboii/matching-game.git
   cd matching-game
   ```

2. **Install the dependencies:**

   ```
   npm install
   ```

3. **Run the development server:**

   ```
   npm run dev
   ```

4. **Open the game in your browser:**

   Navigate to `http://localhost:3000` in your browser to start playing.

## Usage

Once the game is running, click "Play" to start. Flip the cards by clicking on them and try to match all pairs of emojis before the timer runs out. The game can be paused at any time using the Escape key.

### Building for Production

To build the game for production:

```
npm run build
```

The production-ready files will be generated in the `build` directory.

## Game Rules

1. **Objective**: Match all pairs of identical emojis before the timer reaches zero.
2. **How to Play**:

   - Click on a card to flip it and reveal the emoji.
   - Click on another card to find its matching pair.
   - If the two cards match, they remain face up.
   - If they don’t match, they will flip back over after a short delay.
   - The game ends when all pairs are matched or the timer runs out.

3. **Scoring**:
   - The game tracks the time remaining, encouraging you to improve your speed.
   - The faster you match all pairs, the better your performance.

## Customization

You can easily customize the game to suit your preferences:

- **Change the Emoji Set**: Modify the `emoji` array in the `emojis.ts` file to use different emojis.
- **Adjust the Grid Size**: The `size` variable controls the number of cards on the grid. Adjust it to change the difficulty.
- **Modify Timer Duration**: The initial timer duration is set to 60 seconds. You can change the `time` variable to adjust this.

## Development

If you wish to contribute to the development of this game or customize it further, here's a brief overview of the code structure:

- **Main Components**: The core logic is in the `<script lang="ts">` block of the main component.
- **State Management**: The game state (`start`, `playing`, `won`, `lost`, `paused`) is managed using a TypeScript `State` type.
- **Game Logic**: Functions like `createGrid`, `selectCard`, `matchCards`, `startGameTimer`, `resetGame`, `gameWon`, and `gameLost` control the game's flow and functionality.

## Contributing

Contributions are welcome! If you'd like to contribute, please fork the repository and submit a pull request. Whether it's adding new features, fixing bugs, or improving documentation, your contributions are appreciated.

---

Enjoy playing the Emoji Matching Game! 😄

---

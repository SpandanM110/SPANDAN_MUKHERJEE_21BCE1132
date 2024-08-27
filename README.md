

# SPANDAN_MUKHERJEE_21BCE1132

## Project Overview

This project is a chess-inspired game designed with a server-client architecture, utilizing WebSocket for real-time communication. The game is played on a 5x5 grid where two players compete, each controlling characters with unique movement abilities.

```
/chess-game
├── /client
│   ├── index.html       # The main HTML file for the client-side interface
│   ├── styles.css       # CSS file for styling the client-side interface
│   └── chess.js         # JavaScript file for client-side game logic
├── /server
│   └── server.js        # JavaScript file for server-side logic and WebSocket handling
├── package.json         # npm package configuration file
└── package-lock.json    # npm package lock file
```

### Client Directory

- **`index.html`**: Contains the HTML layout for the game board and UI.
- **`styles.css`**: Provides the CSS for the game board, pieces, and overall styling.
- **`chess.js`**: Implements the client-side game logic, including piece movement, turn management, and WebSocket communication.

### Server Directory

- **`server.js`**: Sets up an Express server and handles WebSocket communication for real-time updates between the two players.

## Setup and Installation

To set up the project on your local machine, follow these steps:

1. **Clone the Repository:**
   ```bash
   git clone <repository-url>
   cd server
   ```

2. **Install Dependencies:**
   ```bash
   npm install
   ```

3. **Start the Server:**
   ```bash
   node server.js
   ```

4. **Open the Game:**
   Open your web browser and navigate to `http://localhost:3000` to start playing the game.
   [index.html file]

## How to Play

1. **Select a Piece:**
   Click on a piece to select it. The selected piece will be highlighted.

2. **Move the Piece:**
   Click on a highlighted square to move the selected piece. Only valid moves will be highlighted.

3. **Turn Management:**
   The turn indicator will update to show whose turn it is. Players alternate turns until the game ends.

4. **Winning the Game:**
   The game will notify both players when a player wins based on the rules of the game.


## Dependencies

This project relies on the following dependencies:

- **Express**: A web server framework for Node.js.
- **ws**: A WebSocket library for real-time communication.

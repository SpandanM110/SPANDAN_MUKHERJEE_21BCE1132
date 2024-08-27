

# SPANDAN_MUKHERJEE_21BCE1132

#Github ID: Rovers11 and SpandanM110 (MAIN)

## TO RUN IN LOCAL HOST IMPLEMENT THIS SNIPPET Chess.js code

-----




    // Initial positions of pieces


    document.addEventListener('DOMContentLoaded', function () {
    // Select the board and all boxes
    const board = document.querySelector('.container');
    const boxes = Array.from(document.querySelectorAll('.box'));
    let selectedBox = null; // To keep track of the selected box
    let turn = 'A'; // Starting turn
    
    const initialPositions = {
        'A-P1': [0, 0], 'A-P2': [0, 1], 'A-H1': [0, 2], 'A-H2': [0, 3], 'A-P3': [0, 4],
        'B-P1': [4, 0], 'B-P2': [4, 1], 'B-H1': [4, 2], 'B-H2': [4, 3], 'B-P3': [4, 4]
    };

    // Initialize the board with pieces
    function initializeBoard() {
        for (const piece in initialPositions) {
            const [row, col] = initialPositions[piece];
            const index = row * 5 + col; // Convert row and col to index
            boxes[index].textContent = piece; // Set piece name in the box
            boxes[index].setAttribute('data-piece', piece); // Store piece info in data attribute
        }
    }

    // Check if a move is valid based on piece type and move distance
    function isValidMove(fromIndex, toIndex, piece) {
        const pieceType = piece.split('-')[1]; // Extract piece type
        const fromRow = Math.floor(fromIndex / 5);
        const fromCol = fromIndex % 5;
        const toRow = Math.floor(toIndex / 5);
        const toCol = toIndex % 5;
        const rowDiff = Math.abs(toRow - fromRow);
        const colDiff = Math.abs(toCol - fromCol);

        if (toRow === fromRow) return false; // Can't move onto own starting line

        // Check moves based on piece type
        switch (pieceType) {
            case 'P1':
            case 'P2':
            case 'P3':
                // Pawn moves: one block in any direction
                return (rowDiff === 1 && colDiff <= 1) || (rowDiff <= 1 && colDiff === 1);
            case 'H1':
                // Hero1 moves: two blocks straight
                return (rowDiff === 2 && colDiff === 0) || (rowDiff === 0 && colDiff === 2);
            case 'H2':
                // Hero2 moves: two blocks diagonally
                return (rowDiff === 2 && colDiff === 2);
            default:
                return false;
        }
    }

    // Remove highlight from all boxes
    function clearHighlights() {
        boxes.forEach(box => box.classList.remove('highlight'));
    }

    // Highlight valid moves for the selected piece
    function highlightValidMoves(piece) {
        const index = boxes.indexOf(selectedBox);
        boxes.forEach((box, i) => {
            if (isValidMove(index, i, piece) && isMoveAllowed(index, i)) {
                box.classList.add('highlight');
            }
        });
    }

    // Check if moving to the target box is allowed based on the team
    function isMoveAllowed(fromIndex, toIndex) {
        const fromPiece = boxes[fromIndex].getAttribute('data-piece');
        const toPiece = boxes[toIndex].getAttribute('data-piece');

        if (!toPiece) return true; // Target box is empty

        const toPieceTeam = toPiece[0];
        const fromPieceTeam = fromPiece[0];

        return toPieceTeam !== fromPieceTeam; // Can't move to a box with own team piece
    }

    // Move the piece from one box to another
    function movePiece(fromBox, toBox) {
        toBox.textContent = fromBox.textContent; // Move piece text
        toBox.setAttribute('data-piece', fromBox.getAttribute('data-piece')); // Move piece data
        fromBox.textContent = ''; // Clear source box
        fromBox.removeAttribute('data-piece'); // Remove data from source box
    }

    // Switch turn between players
    function switchTurn() {
        turn = (turn === 'A') ? 'B' : 'A'; // Toggle turn
        document.querySelector('.turn-indicator').textContent = `Player ${turn}'s Turn`; // Update turn indicator
    }

    // Check if there's a winner
    function checkWin() {
        const playerAPieces = boxes.some(box => box.textContent.startsWith('A-'));
        const playerBPieces = boxes.some(box => box.textContent.startsWith('B-'));

        if (!playerAPieces) return 'Player B Wins!';
        if (!playerBPieces) return 'Player A Wins!';
        return null; // No winner yet
    }

    // Handle board clicks
    board.addEventListener('click', function (event) {
        const clickedBox = event.target;

        if (!clickedBox.classList.contains('box')) return; // Clicked outside of boxes

        if (selectedBox) {
            const selectedPiece = selectedBox.getAttribute('data-piece');
            const clickedPiece = clickedBox.getAttribute('data-piece');

            if (clickedBox === selectedBox) {
                // Deselect the selected box
                clearHighlights();
                selectedBox.classList.remove('selected');
                selectedBox = null;
            } else if (clickedBox.classList.contains('highlight')) {
                // Move piece to the highlighted box
                movePiece(selectedBox, clickedBox);
                clearHighlights();
                selectedBox.classList.remove('selected');
                selectedBox = null;
                const winner = checkWin();
                if (winner) {
                    alert(winner); // Display winner
                } else {
                    switchTurn(); // Switch turn if no winner
                }
            } else {
                // Select a new piece if valid
                clearHighlights();
                if (clickedPiece && clickedPiece[0] === turn) {
                    clickedBox.classList.add('selected');
                    selectedBox = clickedBox;
                    highlightValidMoves(selectedPiece);
                }
            }
        } else {
            // Select a piece if none is currently selected
            const piece = clickedBox.getAttribute('data-piece');
            if (piece && piece[0] === turn) {
                clickedBox.classList.add('selected');
                selectedBox = clickedBox;
                highlightValidMoves(piece);
            }
        }
    });

    initializeBoard(); // Initialize the board when the page loads
});


------

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

# VIDEO DEMO OVERALL

https://github.com/user-attachments/assets/94deaecd-a5e9-425e-95ad-e495aa49de0b

## Dependencies

This project relies on the following dependencies:

- **Express**: A web server framework for Node.js.
- **ws**: A WebSocket library for real-time communication.


https://github.com/user-attachments/assets/5584f904-4e4d-4a53-b332-c06d17e97459









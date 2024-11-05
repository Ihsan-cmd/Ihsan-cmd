<!DOCTYPE html>
<html>
<head>
  <title>Sliding Tile Puzzle</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <div id="puzzle-container"></div>
  <script src="script.js"></script>
</body>
</html>
#puzzle-container {
  display: flex;
  flex-wrap: wrap;
  width: 300px;
  height: 300px;
}

.tile {
  width: 100px;
  height: 100px;
  border: 1px solid black;
  text-align: center;
  line-height: 100px;
  font-size: 36px;
  cursor: pointer;
}
const puzzleContainer = document.getElementById('puzzle-container');
const tileSize = 100;
const boardSize = 3; // Adjust the board size as needed

// Create the puzzle tiles
for (let i = 0; i < boardSize * boardSize; i++) {
  const tile = document.createElement('div');
  tile.classList.add('tile');
  tile.textContent = i + 1;
  tile.style.width = tileSize + 'px';
  tile.style.height = tileSize + 'px';
  puzzleContainer.appendChild(tile);
}

// Shuffle the tiles
function shuffleTiles() {
  const tiles = document.querySelectorAll('.tile');
  const shuffledTiles = [...tiles].sort(() => Math.random() - 0.5);
  shuffledTiles.forEach((tile, index) => {
    puzzleContainer.appendChild(tile);
  });
}

// Check if the puzzle is solved
function isSolved() {
  const tiles = document.querySelectorAll('.tile');
  for (let i = 0; i < tiles.length - 1; i++) {
    if (parseInt(tiles[i].textContent) !== i + 1) {
      return false;
    }
  }
  return true;
}

// Event listener for tile clicks
puzzleContainer.addEventListener('click', (event) => {
  const clickedTile = event.target;
  if (clickedTile.classList.contains('tile')) {
    const emptyTileIndex = Array.from(puzzleContainer.children).findIndex(tile => tile.textContent === '');
    const clickedTileIndex = Array.from(puzzleContainer.children).indexOf(clickedTile);

    // Check if the clicked tile is adjacent to the empty tile
    const dx = Math.abs(Math.floor(clickedTileIndex / boardSize) - Math.floor(emptyTileIndex / boardSize));
    const dy = Math.abs(clickedTileIndex % boardSize - emptyTileIndex % boardSize);
    if (dx + dy === 1) {
      // Swap the tiles
      puzzleContainer.insertBefore(clickedTile, puzzleContainer.children[emptyTileIndex]);

      // Check if the puzzle is solved
      if (isSolved()) {
        alert('Congratulations, you solved the puzzle!');
      }
    }
  }
});

// Shuffle the tiles initially
shuffleTiles();

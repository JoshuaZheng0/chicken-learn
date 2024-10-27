<script>
    import { onMount } from "svelte";

    let tiles = [];
    let emptyIndex = 8; // Position of the empty tile (last tile)
    const size = 3; // Size of the puzzle (3x3)
    let initialized = false; // Flag to indicate if tiles have been initialized
    const imageUrls = ["/Images/julie-cat.jpg", "/Images/julie-cat2.jpg", "/Images/julie-cat3.jpg"];
    let imageUrl; // Current image URL
    let won = false; // Flag to indicate if the puzzle is solved
    let count = 1;

    // Initialize the tiles
    function initializeTiles() {
        tiles = Array.from({ length: 8 }, (_, i) => i + 1).concat(null); // Create tiles 1-8 and an empty tile (null)
        shuffleTiles(); // Shuffle the tiles
        selectRandomImage(); // Select a random image
        initialized = true; // Set the flag to true after initialization
        won = false; // Reset the won state when initializing
    }

    // Shuffle tiles randomly
    function shuffleTiles() {
        for (let i = tiles.length - 1; i > 0; i--) {
            const j = Math.floor(Math.random() * (i + 1)); // Random index
            [tiles[i], tiles[j]] = [tiles[j], tiles[i]]; // Swap tiles
        }
        emptyIndex = tiles.indexOf(null); // Update empty index
    }

    // Select a random image from the array
    function selectRandomImage() {
        imageUrl = imageUrls[count%3];
        count++;
    }

    // Check if the tile can move
    function canMove(index) {
        const row = Math.floor(index / size);
        const col = index % size;
        const emptyRow = Math.floor(emptyIndex / size);
        const emptyCol = emptyIndex % size;
        return (
            (Math.abs(row - emptyRow) === 1 && col === emptyCol) || // Adjacent vertically
            (Math.abs(col - emptyCol) === 1 && row === emptyRow) // Adjacent horizontally
        );
    }

    // Move the tile
    function moveTile(index) {
        if (canMove(index)) {
            [tiles[index], tiles[emptyIndex]] = [tiles[emptyIndex], tiles[index]]; // Swap tiles
            emptyIndex = index; // Update empty index
            checkWin(); // Check if the puzzle is won after each move
        }
    }

    function solvePuzzle() {
        tiles = Array.from({ length: 8 }, (_, i) => i + 1).concat(null); // Reset tiles to sorted order
        emptyIndex = tiles.length - 1; // Set emptyIndex to the last position
        won = true; // Set the win state to true when the puzzle is solved
    }

    // Check for winning condition
    function checkWin() {
        if (!initialized) return false; // Only check if initialized
        won = tiles.slice(0, 8).every((tile, index) => tile === index + 1); // Update won state
    }

    function newGame() {
        initializeTiles(); // Reinitialize the tiles and shuffle them
        won = false; // Reset the win state
    }

    onMount(() => {
        initializeTiles(); // Initialize tiles when component mounts
    });
</script>

<style>
    .button-container {
        display: flex; /* Use flexbox for horizontal layout */
        gap: 20px; /* Space between buttons */
        margin-top: 20px; /* Space above the button container */
    }
        button {
        background-color: #fbbf54; /* Yellow background color */
        color: var(--text-color); /* Text color */
        border: none; /* Remove default border */
        border-radius: 5px; /* Rounded corners */
        padding: 10px 20px; /* Padding for the button */
        font-size: 1.2rem; /* Font size */
        font-family: 'Impact', sans-serif; /* Set font to Impact */
        cursor: pointer; /* Pointer cursor on hover */
        transition: background-color 0.3s ease; /* Smooth transition for background color */
    }
    button:hover {
        background-color: val(--button-yellow); 
    }
    button:disabled {
        background-color: grey; /* Grey background when disabled */
        cursor: not-allowed; /* Not-allowed cursor when disabled */
    }
    .puzzle {
        display: grid;
        grid-template-columns: repeat(3, 100px); /* Create a 3x3 grid */
        grid-template-rows: repeat(3, 100px);
        gap: 2px; /* Space between tiles */
    }
    .tile {
        width: 100px; /* Width of each tile */
        height: 100px; /* Height of each tile */
        background-color: #4a90e2; /* Tile color */
        display: flex; /* Center text in tile */
        align-items: center; /* Vertically center */
        justify-content: center; /* Horizontally center */
        font-size: 2rem; /* Font size */
        color: white; /* Text color */
        cursor: pointer; /* Pointer cursor on hover */
    }
    .empty {
        background-color: grey; /* Make empty tile grey */
        cursor: default; /* Default cursor on empty tile */
        background-image: none; /* No background image for empty tile */
    }
    .win-message {
        font-size: 2rem; /* Style for win message */
        color: var(--text-color); /* Color of win message */
        margin-top: 20px; /* Margin above win message */
        font-family: 'Anton', sans-serif; /* Apply Anton font */
        font-style: italic; /* Make the font italic */
    }

</style>

<div class="puzzle">
    {#each tiles as tile, index}
        <!-- svelte-ignore a11y-click-events-have-key-events -->
        <div
            class="tile {tile === null ? 'empty' : ''}" 
            on:click={() => moveTile(index)}
            style="background-image: {tile !== null ? `url('${imageUrl}')` : 'none'}; 
                   background-size: 300px 300px; 
                   background-position: {tile !== null ? `-${((tile - 1) % size) * 100}px -${Math.floor((tile - 1) / size) * 100}px` : '0 0'};"
        >
        </div>
    {/each}
</div>

<div class="button-container">
    <button on:click={solvePuzzle}>Solve</button> <!-- Solve button -->
    <button on:click={newGame}>New Game</button> <!-- New Game button -->
</div>

{#if won}
    <div class="win-message">You Win!</div> <!-- Display win message -->
{/if}

<script>
    import SlidingPuzzle from './SlidingPuzzle.svelte';
</script>

<main>
    <h1>Mr. Paw's Boredom Solver</h1>
    <div class="puzzle-container">
        <SlidingPuzzle />
    </div>
</main>

<style>
    /* Main container styling */
    main {
        text-align: center;
        max-width: 800px;
        background-color: var(--main-yellow); /* Use the main yellow color */
        border-radius: 10px; /* Rounded corners */
        box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2); /* Subtle shadow */
    }

    /* Header styling */
    h1 {
        color: var(--text-color); /* Use the text color from the color scheme */
        font-family: 'Comic Sans MS', cursive, sans-serif; /* Fun font */
        text-shadow: 2px 2px darkorange; /* Use button yellow for a playful effect */
        margin-bottom: 1em; /* Space below the header */
    }

    /* Centering the SlidingPuzzle component */
    .puzzle-container {
        display: flex;
        flex-direction: column;
        justify-content: right; /* Center horizontally */
        align-items: center; /* Center vertically */
        margin: 0 auto; /* Center container in main */
    }
</style>

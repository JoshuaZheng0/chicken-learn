<!-- Home.svelte -->
<script>
    export let setPage; // Import the setPage function from the parent

    function startLearning() {
        setPage('learning'); // Call the setPage function to change the page
    }
</script>

<style>
    @keyframes slideInFromBottom {
        0% {
            transform: translateY(200%); 
            opacity: 0; /* Start transparent */
        }
        100% {
            transform: translateY(25%); /* Move to the original position */
            opacity: 1; /* Fully opaque */
        }
    }
    /* Styling for chicken theme */
    :root {
        --main-yellow: #f9d57e;
        --button-yellow: #ffe066;
        --button-hover-yellow: #fbbf54;
        --text-color: #66492c;
        --background-white: #fff5e1;
    }

    main {
        flex: 1;
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
        color: var(--text-color);
        text-align: center;
        background-size: cover;
        animation: slideInFromBottom 2s ease forwards;
        padding: 2em; /* Added padding for main content */
    }

    h2 {
        font-size: 1.4em; /* Increased size for better visibility */
        margin-top: .5em; /* Changed to em for margin */
        color: var(--text-color);
    }

    .start-button {
        background-color: var(--button-yellow);
        color: var(--text-color);
        border: none;
        padding: .5em 1em; /* Changed to em for padding */
        font-size: 2em; /* Increased font size */
        cursor: pointer;
        border-radius: 1.5em; /* Changed to em for rounded button */
        margin-top: .3em; /* Changed to em for margin */
        transition: background-color 0.3s;
        box-shadow: 0px 0.25em 0.5em rgba(0, 0, 0, 0.2); /* Adjusted shadow to em */
    }

    .start-button:hover {
        background-color: var(--button-hover-yellow);
    }
</style>

<main>
    <header>
        <h2>Join the Fun with Our Flock!</h2>
        <button class="start-button" on:click={startLearning}>Start Learning!</button>
    </header>
</main>

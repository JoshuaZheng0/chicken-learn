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
    }

    h2 {
        font-size: 1.5rem;
        margin-top: 10px;
        color: var(--text-color);
    }

    .start-button {
        background-color: var(--button-yellow);
        color: var(--text-color);
        border: none;
        padding: 15px 30px;
        font-size: 2rem;
        cursor: pointer;
        border-radius: 25px; /* Rounded button */
        margin-top: 20px;
        transition: background-color 0.3s;
        box-shadow: 0px 4px 8px rgba(0, 0, 0, 0.2);
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
<!-- App.svelte -->
<script>
    import { onMount } from 'svelte'; // Import onMount
    import SplineViewer from './SplineViewer.svelte';
    import Home from './Home.svelte';
    import Learning from './Learning.svelte';
    import SlidingPuzzleMain from './SlidingPuzzleMain.svelte';

    let currentPage = 'home';

    function setPage(page) {
        currentPage = page;
    }

    // Scroll to top when the component is mounted
    onMount(() => {
        window.scrollTo(0, 0); // Scroll to the top of the page
    });
</script>

<style>
    .container::-webkit-scrollbar {
        display: none; /* Hide scrollbar */
    }
    @keyframes slideInFromRight {
        0% {
            transform: translateX(100%); /* Start off-screen to the left */
            opacity: 0; /* Start transparent */
        }
        100% {
            transform: translateX(0); /* Move to the original position */
            opacity: 1; /* Fully opaque */
        }
    }

    @keyframes slideInFromLeft {
        0% {
            transform: translateX(-100%); /* Start off-screen to the left */
            opacity: 0; /* Start transparent */
        }
        100% {
            transform: translateX(0); /* Move to the original position */
            opacity: 1; /* Fully opaque */
        }
    }
    :root {
        --main-yellow: #f9d57e;
        --button-yellow: #ffe066;
        --button-hover-yellow: #fbbf54;
        --text-color: #66492c;
    }

    .container {
        display: flex;
        width: 100%;
        height: 100vh;
        overflow-x: hidden; /* Prevent horizontal scrolling */
        overflow-y: hidden;
        background-color: var(--background-white);
        font-family: 'Comic Sans MS', cursive, sans-serif;
        position: relative;
    }

    .background-layer {
        position: absolute;
        top: -20%;
        left: 0;
        width: 100%;
        height: 100%;
        display: flex;
        align-items: center;
        justify-content: center;
        z-index: 0;
        font-family: 'Impact', sans-serif; /* Impact font */
        animation: slideInFromLeft 3s ease forwards;
    }

    /* Text Styling */
    .background-layer h1 {
        font-size: clamp(2rem, 10vw, 8rem); /* Adjusts from 2rem to 8rem based on screen width */
        color: #fbbf54; /* Text color */
        text-align: center; /* Center the text */
        letter-spacing: 5px; /* Slightly reduced spacing for fit */
        padding: 0 2vw; /* Horizontal padding for better spacing on smaller screens */
        word-break: break-word; /* Ensures text wraps if needed */
        overflow-wrap: break-word;
    }

    /* Navigation styling */
    nav {
        display: flex;
        justify-content: left;
        background-color: var(--main-yellow);
        padding: 0px;
    }

    .logo {
        margin: 2vw;
        height: 50px; /* Adjust height as needed */
    }

    nav button {
        margin: 2vw; /* Adjusts margin */
        color: #66492c; /* Text color */
        background-color: transparent; /* Make background transparent */
        border: none; /* No border */
        font-size: 1.2rem; /* Font size */
        cursor: pointer; /* Pointer cursor on hover */
        border-radius: 5px; /* Rounded corners */
        transition: background-color 0.3s; /* Transition effect */
        font-weight: bold;
    }

    nav button:hover {
        background-color: rgba(255, 255, 255, 0.2); /* Optional: Add a hover effect with slight opacity */
    }

    nav button:hover {
        background-color: var(--button-hover-yellow);
    }

    /* Spline Viewer styling */
    .spline-foreground {
        flex: 1;
        display: flex;
        align-items: center;
        justify-content: center;
        z-index: 2; /* Ensure it is on top */
        position: relative; /* Add relative positioning */
    }

    main {
        flex: 1;
        display: flex;
        align-items: center;
        justify-content: center;
        position: relative; /* Ensure this content is above the background */
        z-index: 1;
    }

    /* New style for SlidingPuzzleMain alignment */
	.sliding-puzzle-container {
		margin: 4vw 4vw 0 4vw; /* Top, Right, Bottom, Left */
		display: flex;
		align-items: flex-start;
		justify-content: flex-start;
		width: 100%; 
		height: 100%; 
	}
	.sliding-puzzle-wrapper {
		animation: slideInFromRight 3s ease forwards;
	}
    /* Media query for mobile */
    @media (max-width: 768px) { /* Adjust the max-width as needed */
        .container {
        height: auto; /* Allow height to adjust based on content */
    }
    main {
        flex-direction: column;; /* Ensure main content also stacks vertically */
        height: auto; /* Allow height to adjust based on content */
    }
}


</style>

<nav>
    <img src="favicon.png" alt="Logo" class="logo" />
    <button on:click={() => setPage('home')}>Home</button>
    <button on:click={() => setPage('learning')}>Learning</button>
    <button on:click={() => setPage('sliding')}>Bored?</button>
</nav>

<div class="container">
    <!-- Main Content Area -->
    <main>
        {#if currentPage === 'home'}
            <!-- Background Layer Behind All Content -->
            <div class="background-layer"><h1>LEARN NEW WORDS</h1></div>
            <Home {setPage} />
            <div class="spline-foreground">
                <SplineViewer url="https://prod.spline.design/vGQPzGETFlPq3jEC/scene.splinecode" />
            </div>
        {:else if currentPage === 'learning'}
            <Learning />
        {:else if currentPage === 'sliding'}
            <div class="sliding-puzzle-container">
				<div class="sliding-puzzle-wrapper">
					<SlidingPuzzleMain />
				</div>
				<div class="spline-foreground">
					<SplineViewer url="https://prod.spline.design/AF8qvqIeLt1SgaTG/scene.splinecode" />
				</div>
            </div>
			<div class="background-layer" style="margin-left: 20%;">
				<h1>MEOW MEOW!!</h1>
			</div>
        {/if}
    </main>
</div>

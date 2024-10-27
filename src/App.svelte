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
        height: 100vh; /* Full viewport height */
        overflow-x: hidden; /* Prevent horizontal scrolling */
        overflow-y: auto; /* Allow vertical scrolling */
        background-color: var(--background-white);
        font-family: 'Comic Sans MS', cursive, sans-serif;
        position: relative;
    }


    .background-layer, .background-layer2{
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
    .background-layer2 {
        z-index: 0;
        margin-left: 20%;
    }

    /* Text Styling */
    .background-layer h1,.background-layer2 h1 {
        font-size: clamp(2em, 10vw, 8em); /* Adjusts from 2em to 8em based on screen width */
        color: #fbbf54; /* Text color */
        text-align: center; /* Center the text */
        letter-spacing: 0.1em; /* Adjusted for better fit */
        padding: 0 2%; /* Horizontal padding for better spacing on smaller screens */
        word-break: break-word; /* Ensures text wraps if needed */
        overflow-wrap: break-word;
    }

    /* Navigation styling */
    nav {
        display: flex;
        justify-content: left;
        background-color: var(--main-yellow);
        padding: 0;
    }

    .logo {
        margin: 2%; /* Percentage margin */
        height: 3em; /* Adjust height as needed */
    }

    nav button {
        margin: 2%; /* Adjusts margin */
        color: #66492c; /* Text color */
        background-color: transparent; /* Make background transparent */
        border: none; /* No border */
        font-size: 1.2em; /* Font size */
        cursor: pointer; /* Pointer cursor on hover */
        border-radius: 0.3em; /* Rounded corners */
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
        margin: 4% 4% 0 4%; /* Top, Right, Bottom, Left */
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
    @media (max-width: 380px) { /* Adjust the max-width as needed */
        .container {
            display: flex;
            flex-direction: column; /* Stack items vertically */
            width: 100%;
            min-height: 100vh; /* Minimum height to fill the viewport */
            overflow-y: auto; /* Allow vertical scrolling */
            overflow-x: hidden; /* Prevent horizontal scrolling */
            background-color: var(--background-white);
            font-family: 'Comic Sans MS', cursive, sans-serif;
            position: relative;
        }
        main {
            flex-direction: column; /* Ensure main content also stacks vertically */
            height: auto; /* Allow height to adjust based on content */
        }
        .background-layer {
        top: -10%;
        }
        .background-layer2 {
        top: +10%;
        margin-left: 0;
        }
        .sliding-puzzle-container{
            flex-direction: column;
        }
        
    }
    @media (max-width: 600px) { /* Adjust the max-width as needed */
        @keyframes slideInFromLeft {
            0% {
                transform: translateX(-150%); /* Start off-screen to the left */
                opacity: 0; /* Start transparent */
            }
            100% {
                transform: translateX(0); /* Move to the original position */
                opacity: 1; /* Fully opaque */
            }
        }
        .container {
            display: flex;
            flex-direction: column; /* Stack items vertically */
            width: 100%;
            min-height: 100vh; /* Minimum height to fill the viewport */
            overflow-y: auto; /* Allow vertical scrolling */
            overflow-x: hidden; /* Prevent horizontal scrolling */
            background-color: var(--background-white);
            font-family: 'Comic Sans MS', cursive, sans-serif;
            position: relative;
        }
        main {
            flex-direction: column; /* Ensure main content also stacks vertically */
            height: auto; /* Allow height to adjust based on content */
        }
        .background-layer2 {
        top: 0%;
        margin-left: 0;
        }
        .sliding-puzzle-container{
            flex-direction: column;
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
                <SplineViewer url="https://prod.spline.design/vGQPzGETFlPq3jEC/scene.splinecode" audioUrl="audio/chicken.mp3" />
            </div>
        {:else if currentPage === 'learning'}
            <Learning />
        {:else if currentPage === 'sliding'}
            <div class="sliding-puzzle-container">
                <div class="sliding-puzzle-wrapper" style="position: relative; z-index: 2;">
                    <SlidingPuzzleMain />
                </div>
                <div class="spline-foreground" style="z-index: 2;">
                    <SplineViewer url="https://prod.spline.design/AF8qvqIeLt1SgaTG/scene.splinecode" audioUrl="audio/cat.mp3"/>
                </div>
            </div>
            <div class="background-layer2">
                <h1>MEOW MEOW!!</h1>
            </div>
        {/if}
    </main>
</div>

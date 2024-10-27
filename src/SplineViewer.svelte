<script>
    import { onMount } from 'svelte';

    export let url;
    export let audioUrl;

    let audio;

    onMount(() => {
        const script = document.createElement('script');
        script.type = 'module';
        script.src = 'https://unpkg.com/@splinetool/viewer@1.9.35/build/spline-viewer.js';
        document.head.appendChild(script);

        // Initialize the audio object but don't play it yet
        audio = new Audio(audioUrl);
        // Remove the loop setting to play audio only once
        // audio.loop = true; // Commented out to prevent looping

        // Add a click event listener to the spline viewer
        const splineViewer = document.querySelector('spline-viewer');
        splineViewer.addEventListener('click', playAudio);

        return () => {
            document.head.removeChild(script);
            audio.pause();
            audio.src = '';
            // Remove the event listener on cleanup
            splineViewer.removeEventListener('click', playAudio);
        };
    });

    // Function to play audio
    function playAudio() {
        audio.play().catch(err => {
            console.error('Audio playback failed:', err);
        });
    }
</script>

<main>
    <!-- Render the Spline Viewer component with a specified URL for the 3D scene -->
    <spline-viewer url={url}></spline-viewer>
</main>

<style>
    main {
        width: 100%; /* Set the width of the main container to 100% of its parent */
        height: 100vh; /* Set the height of the main container to the full height of the viewport */
        overflow: hidden; /* Prevent any overflow content from being visible */
        display: flex; /* Use flex to center the spline viewer if needed */
        justify-content: center; /* Center horizontally */
        align-items: center; /* Center vertically */
    }

    spline-viewer {
        width: 100%; /* Set the width of the spline-viewer to 100% of its parent */
        height: 100%; /* Set the height of the spline-viewer to 100% of its parent */
        max-width: 100%; /* Ensure it does not exceed the parent's width */
        max-height: 100%; /* Ensure it does not exceed the parent's height */
    }
</style>

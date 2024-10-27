<script>
    // Import the onMount lifecycle function from the Svelte library
    import { onMount } from 'svelte';

    // Declare a prop for the URL
    export let url;

    // Use the onMount function to run code when the component is mounted
    onMount(() => {
        // Create a new script element to load the Spline Viewer library
        const script = document.createElement('script');

        // Set the script type to 'module' to allow for ES module imports
        script.type = 'module';

        // Specify the source URL of the Spline Viewer library
        script.src = 'https://unpkg.com/@splinetool/viewer@1.9.35/build/spline-viewer.js';

        // Append the script element to the document head to load the library
        document.head.appendChild(script);

        // Return a cleanup function that will run when the component is destroyed
        return () => {
            // Remove the script element from the document head
            document.head.removeChild(script);
        };
    });
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

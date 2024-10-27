<script>
    import { onMount } from 'svelte';

    let randomWord = '';
    let partsofspeech1 = '';
    let partsofspeech2 = '';
    let partsofspeech3 = '';
    let definition = '';
    let definition2 = '';
    let definition3 = '';
    let pronunciation = '';
    let audioUrl = '';
    let audioElement;

    let showNewWord = false; // State to control the visibility of the new word
    let initialLoad = true; // State to track initial load

    onMount(async () => {
        await fetchRandomWord();
    });

    async function fetchRandomWord() {
        try {
            const wordResponse = await fetch('https://api.api-ninjas.com/v1/randomword', {
                headers: { 'X-Api-Key': 'WUrlW7TxH0ZQjS8uiVeNcg==KJpc4rWgP8d1JXZe' }
            });
            if (!wordResponse.ok) throw new Error('Network response was not ok');
            
            const wordData = await wordResponse.json();
            randomWord = wordData.word;
            
            await fetchWordDetails(randomWord);

            if (audioElement) {
                audioElement.load();
            }
            
            // Show the new word after initial load
            if (initialLoad) {
                initialLoad = false; // Update to indicate the initial load has completed
            } else {
                showNewWord = true; // Trigger slide-in for new word
                setTimeout(() => {
                    showNewWord = false; // Reset after animation duration
                }, 1000); // Match with CSS duration
            }
        } catch (error) {
            console.error('Error fetching the random word:', error);
        }
    }

    async function fetchWordDetails(word) {
        try {
            const response = await fetch(`https://api.dictionaryapi.dev/api/v2/entries/en/${word}`);
            const data = await response.json();

            if (data.title === 'No Definitions Found') {
                await fetchRandomWord();
                return;
            }

            const wordInfo = data[0];

            partsofspeech1 = wordInfo.meanings?.[0]?.partOfSpeech || "";
            partsofspeech2 = wordInfo.meanings?.[1]?.partOfSpeech || "";
            partsofspeech3 = wordInfo.meanings?.[2]?.partOfSpeech || "";

            definition = wordInfo.meanings?.[0]?.definitions?.[0]?.definition || "";
            definition2 = wordInfo.meanings?.[1]?.definitions?.[0]?.definition || "";
            definition3 = wordInfo.meanings?.[2]?.definitions?.[0]?.definition || "";

            pronunciation = wordInfo.phonetic 
                || wordInfo.phonetics?.[0]?.text 
                || wordInfo.phonetics?.[1]?.text 
                || wordInfo.phonetics?.[2]?.text 
                || '';
            audioUrl = wordInfo.phonetics?.[0]?.audio || wordInfo.phonetics?.[1]?.audio || wordInfo.phonetics?.[2]?.audio || '';
            
        } catch (error) {
            console.error('Error fetching word details:', error);
        }
    }

    function handleNewWord() {
        fetchRandomWord(); // Fetch new word without triggering animation immediately
    }
</script>

<style>
    @keyframes slideInFromRight {
        0% {
            transform: translateX(150%); /* Start off-screen to the right */
        }
        100% {
            transform: translateX(0); /* Move to the original position */
        }
    }
    @keyframes slideOutToLeft {
        0% {
            transform: translateX(0); /* Start at original position */
        }
        100% {
            transform: translateX(-150%); /* Move out of screen */
        }
    }
    h1 { 
        font-family: 'Impact', sans-serif; /* Impact font */
        font-size: 4vw; /* Adjusts from 2rem to 8rem based on screen width */
        color: #fbbf54; /* Text color */
        letter-spacing: 5px; /* Slightly reduced spacing for fit */
        word-break: break-word; /* Ensures text wraps if needed */
        overflow-wrap: break-word;
     }
    .word-audio-container {
        display: flex;
        align-items: center;
        gap: 15px;
    }
    .word { font-size: 2.5rem; color: #4A90E2; font-weight: bold; }
    .definition, .pronunciation, .partsofspeech { margin-top: 10px; }
    .fetch-button {
        background-color: #4A90E2;
        color: white;
        border: none;
        padding: 10px 20px;
        border-radius: 5px;
        cursor: pointer;
        margin-top: 20px;
    }
    .fetch-button:hover { background-color: #357ABD; }

    /* Animation for the main sections */
    main {
        margin: 2vw;
        position: absolute; /* Fixes it to the viewport */
        top: 0; /* Aligns it to the top */
        width: 50vw;
        max-width: 50vw;
    }
    .slide-in {
        animation: slideInFromRight 1s forwards;
    }
    .slide-out {
        animation: slideOutToLeft 1s forwards;
    }

</style>

<main class:slide-in={!initialLoad && !showNewWord} class:slide-out={showNewWord}>
    <h1>Do You Know This Word?</h1>
    <div class="word-audio-container">
        <div class="word">{randomWord}</div>
        {#if audioUrl}
            <div class="audio-player">
                <audio bind:this={audioElement} controls>
                    <source src={audioUrl} type="audio/mp3" />
                    Your browser does not support the audio element.
                </audio>
            </div>
        {/if}
    </div>
    {#if pronunciation}
        <div class="pronunciation"><strong>Pronunciation:</strong> {pronunciation}</div>
    {/if}
    {#if definition && partsofspeech1}
        <div class="partsofspeech"><strong>(1) Parts of speech:</strong> {partsofspeech1}</div>
        <div class="definition"><strong>(1) Definition:</strong> {definition}</div>
    {/if}
    {#if definition2 && partsofspeech2}
        <div class="partsofspeech"><strong>(2) Parts of speech:</strong> {partsofspeech2}</div>
        <div class="definition"><strong>(2) Definition:</strong> {definition2}</div>
    {/if}
    {#if definition3 && partsofspeech3}
        <div class="partsofspeech"><strong>(3) Parts of speech:</strong> {partsofspeech3}</div>
        <div class="definition"><strong>(3) Definition:</strong> {definition3}</div>
    {/if}
    <button class="fetch-button" on:click={handleNewWord}>Learn New Word</button>
</main>

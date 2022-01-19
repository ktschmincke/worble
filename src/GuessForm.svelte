<script>
    import { createEventDispatcher } from 'svelte';
    import WordRow from './WordRow.svelte';

    const dispatch = createEventDispatcher();

    export let currentGuess = '';
    export let numGuesses = 0;

    $: dispatch('currentGuessChanged', currentGuess);

    function handleSubmit() {
        if (currentGuess.length === 5) {
            dispatch('guess', currentGuess);
        }
    }

    function ordinal(n) {
        switch (n) {
            case 1:
                return 'first';
            case 2:
                return 'second';
            case 3:
                return 'third';
            case 4:
                return 'fourth';
            case 5:
                return 'fifth';
            case 6:
                return 'sixth and final';
        }
    }
</script>

<form on:submit|preventDefault={handleSubmit} class="flow">
    <input id="guess-input" class="visually-hidden" type="text" maxlength="5" bind:value={currentGuess} autocomplete="off">
    <!-- <input id="guess-input" type="text" maxlength="5" on:keydown={handleKeydown} autocomplete="off"> -->
    <label for="guess-input" class="flow">
        <span>Enter your {ordinal(numGuesses + 1)} guess</span>

        <WordRow guess={currentGuess}/>
    </label>
</form>

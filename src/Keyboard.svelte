<script>
    import { createEventDispatcher } from 'svelte';

    const dispatch = createEventDispatcher();

    export let keyMap = {};

    const rows = [
        ['q', 'w', 'e', 'r', 't', 'y', 'u', 'i', 'o', 'p'],
        ['a', 's', 'd', 'f', 'g', 'h', 'j', 'k', 'l'],
        ['z', 'x', 'c', 'v', 'b', 'n', 'm']
    ]

    function keyClicked(key) {
        dispatch('keyClicked', key);
    }

    function enterClicked() {
        dispatch('enterClicked');
    }

    function backspaceClicked() {
        dispatch('backspaceClicked');
    }
</script>

<style>
    .row {
        display: flex;
        gap: 1ch;
        justify-content: center;
    }

    .row > button {
        text-transform: uppercase;
        font-size: 1.2rem;
        min-width: 2rem;
        display: grid;
        place-items: center;
    }

    .row > button[data-state="present"] {
        background-color: var(--present);
    }

    .row > button[data-state="absent"] {
        background-color: var(--absent);
    }

    .row > button[data-state="correct"] {
        background-color: var(--correct);
    }
</style>

<div class="row">
    {#each rows[0] as key}
        <button data-state={keyMap[key]} on:click={() => keyClicked(key)}>{key}</button>
    {/each}
</div>
<div class="row">
    {#each rows[1] as key}
        <button data-state={keyMap[key]} on:click={() => keyClicked(key)}>{key}</button>
    {/each}
</div>
<div class="row">
    <button on:click={enterClicked}>enter</button>
    {#each rows[2] as key}
        <button data-state={keyMap[key]} on:click={() => keyClicked(key)}>{key}</button>
    {/each}
    <button on:click={backspaceClicked}>
        <svg xmlns="http://www.w3.org/2000/svg" height="24" viewBox="0 0 24 24" width="24">
            <path fill="var(--color-tone-1)" d="M22 3H7c-.69 0-1.23.35-1.59.88L0 12l5.41 8.11c.36.53.9.89 1.59.89h15c1.1 0 2-.9 2-2V5c0-1.1-.9-2-2-2zm0 16H7.07L2.4 12l4.66-7H22v14zm-11.59-2L14 13.41 17.59 17 19 15.59 15.41 12 19 8.41 17.59 7 14 10.59 10.41 7 9 8.41 12.59 12 9 15.59z"></path>
        </svg>
    </button>
</div>

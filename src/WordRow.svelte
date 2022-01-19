<script>
  import WordRowLetter from './WordRowLetter.svelte';

  export let guess = '';
  export let state = [];
</script>

<div class="word-row" aria-hidden="true">
  {#each [0, 1, 2, 3, 4] as i}
    <WordRowLetter letter={guess[i] || ''} state={state[i]} />
  {/each}
</div>

<style>
  .word-row {
    display: grid;
    gap: var(--grid-gap);
    grid-template-columns: repeat(5, 1fr);
    justify-content: center;
    margin-left: auto;
    margin-right: auto;
  }
</style>

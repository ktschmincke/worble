<script>
  export let state = '';
  export let letter = '';
</script>

<span class="word-row__letter" data-state={state || 'empty'}>
  {letter}
</span>

<style>
  .word-row__letter {
    display: grid;
    place-items: center;
    aspect-ratio: 1;
    min-height: 55px;
    font-size: 2rem;
    font-weight: 700;
    text-transform: uppercase;
  }

  .word-row__letter[data-state='empty'] {
    border: 1px solid black;
  }

  .word-row__letter[data-state='present'] {
    background-color: var(--present);
    clip-path: polygon(
      30% 0%,
      70% 0%,
      100% 30%,
      100% 70%,
      70% 100%,
      30% 100%,
      0% 70%,
      0% 30%
    );
  }

  .word-row__letter[data-state='absent'] {
    background-color: var(--absent);
  }

  .word-row__letter[data-state='correct'] {
    background-color: var(--correct);
    clip-path: circle(50%);
  }
</style>

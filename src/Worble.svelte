<script>
	import dict from './dict';
	import WordRow from './WordRow.svelte';
	import WordRowLetter from './WordRowLetter.svelte';
	import Keyboard from './Keyboard.svelte';

	const totalGuesses = 6;

	let word = '';
	let currentGuess = '';
	let guesses = [];
	let evaluations = [];
	let keyMap = {};
	let win = false;

	$: maxedGuesses = guesses.length === totalGuesses;

	function checkGuess() {
		const isWord = dict.indexOf(currentGuess) !== -1;
		if (!isWord) {
			console.log(`'${currentGuess}' is not a word`);
			return;
		}

		let evaluation = [];
		for (let i = 0; i < 5; i++) {
			const guessLetter = currentGuess[i];

			if (guessLetter === word[i]) {
				evaluation.push('correct');
				keyMap = {...keyMap, [guessLetter]: 'correct'};
			} else if (word.indexOf(guessLetter) !== -1) {
				evaluation.push('present');
				keyMap = {...keyMap, [guessLetter]: 'present'};
			} else {
				evaluation.push('absent');
				keyMap = {...keyMap, [guessLetter]: 'absent'};
			}
		}

		guesses = [...guesses, currentGuess];
		evaluations = [...evaluations, evaluation];

		if (currentGuess === word) {
			win = true;
		}

		currentGuess = '';
	}

	function keyClicked(key) {
		if (currentGuess.length < 5) {
			currentGuess = currentGuess + key;
		}
	}

	function backspaceClicked() {
		currentGuess = currentGuess.substring(0, currentGuess.length - 1);
	}

	function currentGuessChanged(event) {
		currentGuess = event.detail;
	}

	function handleWindowKeydown(e) {
		if (e.target.tagName === 'INPUT') {
			return;
		}

		if (e.key === 'Enter') {
			checkGuess();
		} else if (e.key === 'Backspace') {
			backspaceClicked();
		} else if (e.key.length === 1 && /[a-zA-Z]/.test(e.key)) {
			keyClicked(e.key);
		}
	}

	function resetGame() {
		word = dict[Math.floor(Math.random() * dict.length)];
		guesses = [];
		evaluations = [];
		currentGuess = '';
		win = false;
		keyMap = {};
		console.log(`word is ${word}`);
	}

	resetGame();
</script>

<svelte:head>
	<title>worble</title>
</svelte:head>

<svelte:window on:keydown={handleWindowKeydown}/>

<main>
	<h1>WORBLE</h1>

	<div class="flow game-board" style="--flow-space: var(--grid-gap)">
		{#each guesses as guess, i}
			<WordRow guess={guess} state={evaluations[i]}/>
		{/each}

		{#if !maxedGuesses && !win}
			<WordRow guess={currentGuess}/>
		{/if}

		{#if maxedGuesses && !win}
			<p>YOU'VE BEEN WORBLED!!!!!!!!!!!! The word was '{word}'.</p>
		{/if}

		{#if win}
			<p>Congrats! You got it right!</p>
		{/if}

		{#if maxedGuesses || win}
			<button on:click={resetGame}>Play again</button>
		{/if}

	</div>

	<Keyboard
		keyMap={keyMap}
		on:keyClicked={(e) => keyClicked(e.detail)}
		on:enterClicked={checkGuess}
		on:backspaceClicked={backspaceClicked}
	/>
</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 40rem;
		margin: 0 auto;
		display: flex;
		flex-direction: column;
		height: 100%;
	}

	.game-board {
		flex: 1;
	}

	h1 {
		text-transform: uppercase;
		font-size: 2em;
		font-weight: 900;
		margin: .5rem 0 2rem;
		transform: rotate(-10deg)
	}

	:global(.flow > * + *) {
		margin-top: var(--flow-space, 1rem);
	}

	:global(.visually-hidden) {
		clip: rect(0 0 0 0);
		clip-path: inset(50%);
		height: 1px;
		overflow: hidden;
		position: absolute;
		white-space: nowrap;
		width: 1px;
	}

	:global(:root) {
		--present: #b59f3b;
		--absent: #939598;
		--correct: #538d4e;
		--grid-gap: 0.5ch;
	}

	:global(*, *::before, *::after) {
		box-sizing: border-box;
	}
</style>

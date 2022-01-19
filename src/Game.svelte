<script>
    import dict from './dict';
	import WordRow from './WordRow.svelte';
	import Keyboard from './Keyboard.svelte';

    const totalGuesses = 6;

	let word = '';
	let currentGuess = '';
	let guesses = [];
	let evaluations = [];
	let keyMap = {};
	let win = false;
    let badGuess = '';

	$: maxedGuesses = guesses.length === totalGuesses;
    $: currentGuess = currentGuess.toLowerCase();

	function checkGuess() {
		const isWord = dict.indexOf(currentGuess) !== -1;
		if (!isWord) {
            badGuess = currentGuess;
			setTimeout(() => {
                badGuess = '';
            }, 5000)
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

<svelte:window on:keydown={handleWindowKeydown}/>

<main class="flow">
	<h1>WORBLE</h1>

	<div class="flow game-board">
		{#each guesses as guess, i}
			<WordRow guess={guess} state={evaluations[i]}/>
		{/each}

		{#if !maxedGuesses && !win}
			<WordRow guess={currentGuess}/>

            {#if badGuess}
                <p style="text-align: center">Hey! '{badGuess}' isn't a word!</p>
            {/if}
		{/if}

	</div>

    {#if maxedGuesses || win}
        <div class="results">
            {#if maxedGuesses && !win}
                <p>YOU'VE BEEN</p>
                <p class="worbled">WORBLED!!!!!!!!!!!!</p>
                <p>The word was '{word}'.</p>
            {/if}
            {#if win}
                <p>Congrats! You got it right!</p>
            {/if}
            {#if maxedGuesses || win}
                <button on:click={resetGame}>Play again</button>
            {/if}
        </div>
    {:else}
        <div class="keyboard">
            <Keyboard
                keyMap={keyMap}
                on:keyClicked={(e) => keyClicked(e.detail)}
                on:enterClicked={checkGuess}
                on:backspaceClicked={backspaceClicked}
            />
        </div>
    {/if}
</main>

<style>
    main {
		max-width: 30rem;
		margin: 0 auto;
		display: flex;
		flex-direction: column;
        align-items: center;
		height: 100%;
	}

    .game-board {
        --flow-space: var(--grid-gap);
        width: 100%;
    }

    @media (max-height: 888px) {
        .game-board {
            padding: 0 3rem;
        }
    }

	h1 {
		text-transform: uppercase;
		font-size: 2em;
		font-weight: 900;
		margin: .5rem 0 2rem;
		transform: rotate(-4deg)
	}

    .results {
        text-align: center;
        margin-top: 2rem;
    }

    .worbled {
        animation: worbled .75s infinite ease-in-out alternate;
        font-size: 1.2em;
        font-weight: bold;
        margin: 1rem auto;
        width: fit-content;
    }

    .keyboard {
        margin-top: auto;
        padding-top: 1rem;
        width: 100%;
    }

    @keyframes worbled {
        10% {
            transform: scale(1.1) rotate(3deg);
        }

        20% {
            transform: scale(1.2) rotate(-3deg);
        }

        30% {
            transform: scale(1.3) rotate(3deg);
        }

        40% {
            transform: scale(1.4) rotate(-3deg);
        }

        50% {
            transform: scale(1.5) rotate(3deg);
        }

        60% {
            transform: scale(1.6) rotate(-3deg);
        }

        70% {
            transform: scale(1.7) rotate(3deg);
        }

        80% {
            transform: scale(1.8) rotate(-3deg);
        }

        90% {
            transform: scale(1.9) rotate(3deg);
        }

        100% {
            transform: scale(2) rotate(-3deg);
        }
    }
</style>

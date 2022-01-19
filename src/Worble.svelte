<script>
	import Game from './Game.svelte';
</script>

<svelte:head>
	<title>worble</title>
	<link rel="apple-touch-icon" sizes="180x180" href="/assets/apple-touch-icon.png">
	<link rel="icon" type="image/png" sizes="32x32" href="/assets/favicon-32x32.png">
	<link rel="icon" type="image/png" sizes="16x16" href="/assets/favicon-16x16.png">
	<link rel="manifest" href="/assets/site.webmanifest">
</svelte:head>

<Game/>

<style>
	:global(.flow > * + *) {
		margin-top: var(--flow-space, 1rem);
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

<script>
	import hereIAm from './data/hereIAm.js';
	import { onMount } from 'svelte'; 
	
	export let title;

	const words = [
		'a', 
		{
			word: 'wet', 
			xDelay: 1000
		},
		['mar', 'ket'],
		'smile', 
		'on', 
		'your', 
		['for', 'get', 'ta', 'ble'], 
		'face'
	];
	const wordCount = words.length;
	let wordIndex = 0;

	let word = '';
	let syllableIndex = 1;

	function magnifyWords() {
		let nextExpression = words[wordIndex],
			delayBeforeNextWord = 1000;

		if (typeof nextExpression === 'object' && !Array.isArray(nextExpression)) {
			delayBeforeNextWord += nextExpression.xDelay;
			nextExpression = nextExpression.word;
		}

		if (Array.isArray(nextExpression)) {
			let syllableCount = nextExpression.length;
			word = nextExpression.slice(0, syllableIndex).join('');
			if (syllableIndex < syllableCount) { 
				syllableIndex++;
				delayBeforeNextWord = 500;
			} else {
				syllableIndex = 1;
				wordIndex++;
				delayBeforeNextWord = 1000;
			}
		} else {
			word = nextExpression;
			if (wordIndex + 1 < wordCount) wordIndex++;
		} 

		setTimeout(() => magnifyWords(), delayBeforeNextWord);
	}

	onMount(() => {
		magnifyWords();
	})
</script>

<svelte:head>
  <title>{title}</title>
</svelte:head>

<main>
	<div class='magnifier'>
		{word}
	</div>
</main>

<style>
	main {
		display: flex;
		align-items: center;
		width: 100%;
		height: 100%;
		font-family: 'EB Garamond', serif;
		/* border: solid 1px black; */
	}
	
	.magnifier {
		width: 100%;
		font-size: 80px;
		text-align: center;
		/* border: dotted red 1px; */
	}
</style>
<script>
	import hereIAm from './data/hereIAm.js';
	import { onMount } from 'svelte'; 
	
	export let title;

	let firstVerse = hereIAm[0];
	let lines = [];
	firstVerse.couplets.forEach(couplet => {
		lines.push(couplet.a);
		lines.push(couplet.b);
	});

	const getRandomInt = function(max, min = 0) {
  	return Math.floor(Math.random() * (max - min + 1) + min);
	};
	
	const commaReg = /,/, dashReg = /[–-]/;
	let parsedLines = [];

	lines.forEach(line => {
		let splitLine = line.split(' ');
		let parsedLine = [];
			splitLine.forEach((word, i) => {
				let parsedWord = word;
				if (commaReg.test(word)) {
					parsedWord = {
						word: parsedWord.slice(0, parsedWord.length - 1),
						xDelay: 1000,
						note: 'comma'
					}
				} else if (splitLine.length === i + 1) { 
					parsedWord = {
						word: parsedWord,
						xDelay: 2000,
						note: 'end of line'
					}
				} 
				parsedLine.push(parsedWord);
			});
		parsedLines.push(parsedLine); 
	});

	console.log(parsedLines);

	const words = parsedLines.flat();
	const wordCount = words.length;
	let wordIndex = 0;

	let word = '';

	function magnifyWords() {
		let nextExpression = words[wordIndex], letterCount = nextExpression.length,
			delayBeforeNextWord = getRandomInt(10, 5) * 100;

		if (letterCount > 5) delayBeforeNextWord += (Math.floor(letterCount / 5) * 300);

		if (typeof nextExpression === 'object' && !Array.isArray(nextExpression)) {
			delayBeforeNextWord += nextExpression.xDelay;
			nextExpression = nextExpression.word;
		}

		word = nextExpression;
		if (wordIndex + 1 < wordCount) wordIndex++;

		setTimeout(() => magnifyWords(), delayBeforeNextWord);
	}

	onMount(() => {
		setTimeout(() => magnifyWords(), 5000);
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
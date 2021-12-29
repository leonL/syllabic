<script>
	import hereIAm from './data/hereIAm.js';
	import { onMount } from 'svelte'; 
	import Carousel from 'svelte-carousel';
	
	export let title;
	let carousel;

	let firstVerse = hereIAm[0];
	let firstCouplet = firstVerse.couplets[0];
	let firstCoupletWords = [...firstCouplet.a.split(' '), ...firstCouplet.b.split(' ')];

	console.log(firstCoupletWords);
</script>

<svelte:head>
  <title>{title}</title>
</svelte:head>

<main>
	<Carousel bind:this={carousel} dots={false} arrows={false}>
		{#each firstCoupletWords as word}
			<div class='magnifier'>{ word }</div>
		{/each}
	</Carousel>
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
		font-size: 60px;
		text-align: center;
		/* border: dotted red 1px; */
	}
</style>
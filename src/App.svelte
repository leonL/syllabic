<script>
	import hereIAm from './data/hereIAm.js';
	import { onMount } from 'svelte'; 
	import Carousel from 'svelte-carousel';
	
	export let title;
	let carousel, carouselIndex;

	function goToPrevPage() {
		carousel.goToPrev({ animated: false });
	}

	function goToNextPage() {
		carousel.goToNext({ animated: false });
	}

	let firstVerse = hereIAm[0];
	let firstCouplet = firstVerse.couplets[0];
	let firstCoupletWords = [...firstCouplet.a.split(' ')];

	console.log(firstCoupletWords);
</script>

<svelte:head>
  <title>{title}</title>
</svelte:head>

<main>
	<Carousel bind:this={carousel} arrows={false} swiping={false} duration={1000} let:currentPageIndex>
		{#each firstCoupletWords as word}
			<div class='magnifier'>{ word }</div>
		{/each}
		<div slot="dots">
			{#each firstCoupletWords as word, i}
				<span class="word {currentPageIndex === i ? 'active' : ''}">{ word } </span>
			{/each}
		</div>
	</Carousel>
	<div class="navigation">
		<div class="step" on:click={goToPrevPage}></div>
		<div class="step" on:click={goToNextPage}></div>
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
		font-size: 60px;
		text-align: center;
	}

	.word {
		font-size: 18px;
	}

	.active {
		font-weight: 800;
	}

	.navigation {
		position: absolute;
		top: 0;
		bottom: 0;
		left: 0;
		right: 0;
		display: flex;
		flex-direction: row;
		/* border: black 1px solid; */
	}
	
	.step {
		height: 100%;
		flex-grow: 1;
		/* border: dotted red 1px; */
	}
</style>
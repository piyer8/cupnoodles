<script>
	import RamenHero from '$lib/RamenHero.svelte';
	let pageEl = $state(null);
</script>

<div class="page" bind:this={pageEl}>
	<RamenHero scrollRoot={pageEl} />

	<section class="snap-section" style="background-color: #3d2b1f; color: #f5e6d3;">
		<p>The story begins — placeholder text</p>
	</section>

	<section class="snap-section" style="background-color: #3d2b1f; color: #f5e6d3;">
		<p>The story continues — placeholder text</p>
	</section>

	<section class="snap-section" style="background-color: #3d2b1f; color: #f5e6d3;">
		<p>The story ends — placeholder text</p>
	</section>
</div>

<style>
	:global(body) {
		margin: 0;
		padding: 0;
	}

	.page {
		height: 100vh;
		overflow-y: scroll;
		scroll-snap-type: y mandatory;
	}

	/* The hero section gets its snap point so scrolling INTO it locks to top */
	:global(.hero) {
		scroll-snap-align: start;
	}

	.snap-section {
		height: 100vh;
		scroll-snap-align: start;
		display: flex;
		align-items: center;
		justify-content: center;
		font-size: 1.5rem;
	}
</style>

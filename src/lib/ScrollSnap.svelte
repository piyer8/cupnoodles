<script>
	let { children } = $props();
</script>

<div class="scroll-container">
	{@render children()}
</div>

<style>
	.scroll-container {
		height: 100vh;
		overflow-y: scroll;
		scroll-snap-type: y mandatory;
	}
</style>

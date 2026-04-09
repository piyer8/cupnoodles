<script>
	let { frames = ['/images/temp_ramen.avif'], scrollRoot = null } = $props();

	let heroEl = $state(null);
	let bgColor = $state('#f5e6d3');
	let currentFrame = $state(0);
	let progress = $state(0);

	// Opacity for the two text panels, derived from scroll progress
	// Panel A (initial headline): visible 0–0.25, fades out by 0.4
	let panelAOpacity = $derived(
		progress < 0.25 ? 1 : Math.max(0, 1 - (progress - 0.25) / 0.15)
	);
	// Panel B (secondary heading + text): fades in 0.5–0.65
	let panelBOpacity = $derived(
		progress < 0.5 ? 0 : Math.min(1, (progress - 0.5) / 0.15)
	);

	const colorStops = [
		{ at: 0,    color: '#f5e6d3' },
		{ at: 0.4,  color: '#e8c49a' },
		{ at: 0.75, color: '#3d2b1f' },
	];

	function getColorAt(p) {
		let color = colorStops[0].color;
		for (const stop of colorStops) {
			if (p >= stop.at) color = stop.color;
		}
		return color;
	}

	function onScroll() {
		if (!heroEl) return;
		const { top, height } = heroEl.getBoundingClientRect();
		const scrolled = -top;
		const scrollable = height - window.innerHeight;
		progress = Math.min(1, Math.max(0, scrolled / scrollable));
		currentFrame = Math.min(frames.length - 1, Math.floor(progress * frames.length));
		bgColor = getColorAt(progress);
	}

	$effect(() => {
		const root = scrollRoot ?? window;
		root.addEventListener('scroll', onScroll, { passive: true });
		return () => root.removeEventListener('scroll', onScroll);
	});
</script>

<div class="hero" bind:this={heroEl}>
	<div class="stage" style="background-color: {bgColor}">
		<div class="left">
			<img
				src={frames[currentFrame]}
				alt="Ramen cup animation"
				class="cup"
			/>
		</div>

		<div class="right">
			<!-- Panel A: initial headline, fades out as user scrolls -->
			<div class="panel" style="opacity: {panelAOpacity}; pointer-events: {panelAOpacity === 0 ? 'none' : 'auto'}">
				<h1>The History<br />of Ramen</h1>
			</div>

			<!-- Panel B: secondary content, fades in mid-scroll -->
			<div class="panel" style="opacity: {panelBOpacity}; pointer-events: {panelBOpacity === 0 ? 'none' : 'auto'}">
				<h2>A noodle born<br />from necessity</h2>
				<p>Placeholder body text about ramen origins and history goes here.</p>
			</div>
		</div>
	</div>
</div>

<style>
	.hero {
		height: 400vh;
		position: relative;
	}

	.stage {
		position: sticky;
		top: 0;
		height: 100vh;
		display: flex;
		align-items: center;
		transition: background-color 0.4s ease;
	}

	.left {
		flex: 1;
		display: flex;
		align-items: center;
		justify-content: center;
		height: 100%;
	}

	.right {
		flex: 1;
		position: relative;
		display: flex;
		align-items: center;
		justify-content: center;
		height: 100%;
	}

	.panel {
		position: absolute;
		display: flex;
		flex-direction: column;
		gap: 1rem;
		padding: 2rem;
		transition: opacity 0.1s ease;
	}

	.cup {
		max-height: 60vh;
		max-width: 45vw;
		object-fit: contain;
		user-select: none;
		pointer-events: none;
	}

	h1 {
		font-size: clamp(2rem, 4vw, 3.5rem);
		line-height: 1.1;
		margin: 0;
		font-weight: 700;
	}

	h2 {
		font-size: clamp(1.5rem, 3vw, 2.5rem);
		line-height: 1.2;
		margin: 0;
		font-weight: 600;
	}

	p {
		font-size: 1rem;
		line-height: 1.6;
		max-width: 36ch;
		margin: 0;
	}
</style>

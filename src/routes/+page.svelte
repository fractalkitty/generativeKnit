<script lang="ts">
	import RadioGroup from '$lib/components/RadioGroup.svelte';
	import CheckboxGroup from '$lib/components/CheckboxGroup.svelte';
	import PatternGrid from '$lib/components/PatternGrid.svelte';
	import DownloadButton from '$lib/components/DownloadButton.svelte';
	import '$lib/styles/theme.css';
	import '$lib/styles/fonts.css';

	let mode = 'lacy';
	let canvas: HTMLCanvasElement;
	let refreshKey = 0;
	let patternCanvas: HTMLCanvasElement;
	let nPanel = '6';
	let increases = ['yo', 'm1l', 'm1r'];
	let decreases = ['k2tog', 'ssk'];
	let isLoading = false;
	let wyrmImg;

	function handleMouseEnter() {
		wyrmImg.src = `/wyrm.gif?${new Date().getTime()}`;
	}

	function handleMouseLeave() {
		wyrmImg.src = '/wyrmstatic.png';
	}

	const modes = ['lacy', 'knit-purl'];
	const nPanels = ['4', '5', '6'];
	const increaseOptions = ['yo', 'm1l', 'm1r'];
	const decreaseOptions = ['k2tog', 'ssk'];

	function handleGenerate() {
		isLoading = true;

		console.log('Generate clicked, canvas:', patternCanvas);
		if (!patternCanvas) {
			console.error('No canvas reference');
			return;
		}
		const imageData = patternCanvas.toDataURL('image/png');
		const link = document.createElement('a');
		link.href = imageData;
		link.download = 'knitting_pattern.png';
		link.click();
	}

	async function handleRefresh() {
		isLoading = true;
		refreshKey += 1;
	}
</script>

<main class="container mx-auto max-w-[95vw] bg-[var(--color-background)] px-4 py-6 md:max-w-4xl">
	<header class="mb-8 flex items-center justify-start gap-8">
		<div class="wyrm-img relative h-40 w-40">
			<img
				bind:this={wyrmImg}
				src="/wyrmstatic.png"
				class="absolute h-40 w-40 transition-opacity duration-300 hover:opacity-90"
				alt="A coiled green Wyrm with a blue hat - sitting coiled. It's tongue moves."
				loading="lazy"
				on:mouseenter={handleMouseEnter}
				on:mouseleave={handleMouseLeave}
			/>
		</div>
		<div class="text-center">
			<h1 class="mb-2 text-4xl font-[var(--font-heading)] text-[var(--color-text)]">Wyrm Bean</h1>
			<p class="text-[var(--color-text)]/80 font-[var(--font-body)]">
				Algorithmic beanies for the modern dragon.
			</p>
		</div>
	</header>
	<section class="mb-8">
		<div class="grid grid-cols-1 gap-4 sm:grid-cols-2 lg:grid-cols-4">
			<div class="rounded-lg border-2 border-[var(--color-primary)] bg-[var(--color-primary)] p-3">
				<h3 class="mb-2 font-medium text-[var(--color-text)]">Mode</h3>
				<div class="flex justify-center">
					<RadioGroup
						name="mode"
						bind:selected={mode}
						options={modes}
						activeColor="var(--color-primary-btn)"
					/>
				</div>
			</div>

			<div
				class="rounded-lg border-2 border-[var(--color-secondary)] bg-[var(--color-secondary)] p-3"
			>
				<h3 class="mb-2 font-medium text-[var(--color-text)]">Panels</h3>
				<div class="flex justify-center">
					<RadioGroup
						name="nPanel"
						bind:selected={nPanel}
						options={nPanels}
						activeColor="var(--color-secondary-btn)"
					/>
				</div>
			</div>

			<div
				class="rounded-lg border-2 border-[var(--color-tertiary)] bg-[var(--color-tertiary)] p-3 {mode !==
				'lacy'
					? 'opacity-50'
					: ''}"
			>
				<h3 class="mb-2 font-medium text-[var(--color-text)]">Increases</h3>
				<div class="flex justify-center">
					<CheckboxGroup
						name="increases"
						bind:selected={increases}
						options={increaseOptions}
						disabled={mode !== 'lacy'}
						activeColor="var(--color-tertiary-btn)"
						required
					/>
				</div>
			</div>

			<div
				class="rounded-lg border-2 border-[var(--color-quaternary)] bg-[var(--color-quaternary)] p-3 {mode !==
				'lacy'
					? 'opacity-50'
					: ''}"
			>
				<h3 class="mb-2 font-medium text-[var(--color-text)]">Decreases</h3>
				<div class="flex justify-center">
					<CheckboxGroup
						name="decreases"
						bind:selected={decreases}
						options={decreaseOptions}
						disabled={mode !== 'lacy'}
						activeColor="var(--color-quaternary-btn)"
						required
					/>
				</div>
			</div>
		</div>
	</section>
	<div class="mb-10 mt-8 flex justify-center gap-4">
		<button
			class="rounded-md bg-[var(--color-text)] px-4 py-2 text-white transition-opacity hover:opacity-90"
			on:click={handleRefresh}
		>
			Randomize
		</button>
		<DownloadButton onGenerate={handleGenerate}>Download Pattern</DownloadButton>
	</div>
	<section class="rounded-lg border bg-gray-50 p-4">
		<h2 class="mb-4 text-center font-medium">Printable</h2>
		<div class="overflow-x-auto rounded-b bg-white">
			<div class="flex min-w-fit items-center justify-center p-4">
				<PatternGrid
					bind:canvas={patternCanvas}
					{mode}
					nPanel={parseInt(nPanel)}
					{increases}
					{decreases}
					{refreshKey}
				/>
			</div>
		</div>
	</section>
</main>

<style>
	.wyrm-img {
		background-image: url('/wyrmstatic.png');
	}
	.wyrm-img:hover {
		background-image: url('/wyrm.gif');
	}
</style>

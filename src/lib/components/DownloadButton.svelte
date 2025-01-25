<script lang="ts">
	// This is a "callback prop" that the parent will pass in
	export let onGenerate: () => void;
	export let canvas: HTMLCanvasElement;
</script>

<button
	on:click={onGenerate}
	class="rounded-md bg-[var(--color-text)] px-4 py-2 text-white transition-opacity hover:opacity-90"
>
	<slot />
</button>

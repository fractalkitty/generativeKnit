<script lang="ts">
	export let options: string[];
	export let selected: string;
	export let name: string;
	export let activeColor: string = '#91b0bf';
</script>

<div class="flex gap-3">
	{#each options as option}
		<label class="flex cursor-pointer items-center">
			<input
				type="radio"
				{name}
				value={option}
				bind:group={selected}
				class="peer hidden"
				required
			/>
			<span
				class="rounded-md border px-3 py-1 transition-colors hover:bg-gray-50 peer-checked:text-white"
				style="--active-color: {activeColor}; border-color: var(--active-color); background-color: {selected ===
				option
					? activeColor
					: 'transparent'}"
			>
				{option}
			</span>
		</label>
	{/each}
</div>

<script lang="ts">
	export let options: string[];
	export let selected: string[];
	export let name: string;
	export let disabled = false;
	export let required = false;
	export let activeColor: string = '#e184ab';

	function handleChange(option: string) {
		if (selected.includes(option)) {
			if (selected.length > 1) {
				selected = selected.filter((item) => item !== option);
			}
		} else {
			selected = [...selected, option];
		}
	}
</script>

<div class="flex gap-3">
	{#each options as option}
		<label
			class="flex cursor-pointer items-center {disabled ? 'cursor-not-allowed opacity-50' : ''}"
		>
			<input
				type="checkbox"
				{name}
				value={option}
				checked={selected.includes(option)}
				on:change={() => handleChange(option)}
				{disabled}
				{required}
				class="peer hidden"
			/>
			<span
				class="rounded-md border px-3 py-1 transition-colors peer-checked:text-white"
				style="--active-color: {activeColor}; border-color: var(--active-color); background-color: {selected.includes(
					option
				)
					? activeColor
					: 'transparent'}; color: {selected.includes(option) ? 'white' : 'inherit'}"
			>
				{option}
			</span>
		</label>
	{/each}
</div>

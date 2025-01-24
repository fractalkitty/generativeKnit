<script lang="ts">
  export let options: string[];
  export let selected: string;
  export let name: string;
</script>

<div class="flex gap-6">
  {#each options as option}
    <label class="flex items-center cursor-pointer">
      <input type="radio" {name} value={option} bind:group={selected} 
        class="hidden peer" required />
      <span class="px-4 py-2 border rounded-lg peer-checked:bg-blue-500 
        peer-checked:text-white transition-colors">{option}</span>
    </label>
  {/each}
</div>
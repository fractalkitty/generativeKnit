
<script lang="ts">
 import RadioGroup from '$lib/components/RadioGroup.svelte';
 import CheckboxGroup from '$lib/components/CheckboxGroup.svelte';
 import PatternGrid from '$lib/components/PatternGrid.svelte';
 let mode = 'lacy';
 let panelSize = '20';
 let increases = ['yo', 'm1l', 'm1r'];
 let decreases = ['k2tog', 'ssk'];
 
 const modes = ['lacy', 'knit-purl'];
 const panelSizes = ['10', '20', '25'];
 const increaseOptions = ['yo', 'm1l', 'm1r'];
 const decreaseOptions = ['k2tog', 'ssk'];
</script>

<main class="max-w-2xl mx-auto p-8">
 <div class="grid grid-cols-1 gap-8">
   <section class="space-y-8">
     <div class="space-y-2">
       <h3 class="font-medium">Mode</h3>
       <RadioGroup name="mode" bind:selected={mode} options={modes} />
     </div>
     
     <div class="space-y-2">
       <h3 class="font-medium">Panel Size</h3>
       <RadioGroup name="panelSize" bind:selected={panelSize} options={panelSizes} />
     </div>

     {#if mode === 'lacy'}
       <div class="space-y-2">
         <h3 class="font-medium">Increases</h3>
         <CheckboxGroup name="increases" bind:selected={increases} options={increaseOptions} required />
       </div>

       <div class="space-y-2">
         <h3 class="font-medium">Decreases</h3>
         <CheckboxGroup name="decreases" bind:selected={decreases} options={decreaseOptions} required />
       </div>
     {/if}
   </section>
   
   <section class="border rounded-lg p-4 bg-gray-50">
  <h2 class="font-medium mb-4 text-center">Preview</h2>
  <div class="bg-white rounded-b flex justify-center items-center">
    <PatternGrid 
      {mode}
      panelSize={parseInt(panelSize)}
      {increases}
      {decreases}
    />
  </div>
</section>
 </div>
</main>
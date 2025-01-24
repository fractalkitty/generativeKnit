<script lang="ts">
  export let type: 'knit' | 'purl' | 'yo' | 'k2tog' | 'ssk' | 'm1l' | 'm1r' | 's2kp2';
  export let radius = 4; // Default circle radius
  const width = 20;
  const height = 20;
  const symbols = {
    knit: 'M 0,0 L 20,0 L 20,20 L 0,20 Z',
    k2tog: 'M 0,0 L 20,0 L 20,20 L 0,20 Z M 17,3 L 3,17',
    ssk: 'M 0,0 L 20,0 L 20,20 L 0,20 Z M 3,3 L 17,17',
    m1l: 'M 0,0 L 20,0 L 20,20 L 0,20 Z M 5,5 L 15,15 M 10,10 L 15,5',
    m1r: 'M 0,0 L 20,0 L 20,20 L 0,20 Z M 15,5 L 5,15 M 10,10 L 5,5',
    s2kp2: 'M 0,0 L 20,0 L 20,20 L 0,20 Z M 10,5 L 10,15 M 10,5 L 15,15 M 10,5 L 5,15'
  };
</script>

<svg width={20} height={20} viewBox="0 0 20 20" style="display: block;">
  <!-- Square -->
  <path 
    d="M 0,0 L 20,0 L 20,20 L 0,20 Z" 
    fill="white" 
    stroke="black" 
    stroke-width="2" 
  />
  <!-- Circle for 'purl' -->
  {#if type === 'purl'}
    <circle cx="10" cy="10" r={radius} fill="black" stroke="none" />
  {/if}
  <!-- Circle for 'yo' -->
  {#if type === 'yo'}
    <circle cx="10" cy="10" r={radius} fill="none" stroke-width=2 stroke="black" />
  {/if}
  <!-- Other symbols -->
  {#if type !== 'purl' && type !== 'yo'}
    <path d={symbols[type]} fill="none" stroke-width=2 stroke="black" />
  {/if}
</svg>
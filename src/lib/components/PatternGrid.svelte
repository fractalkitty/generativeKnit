<script lang="ts">
  import KnittingSymbol from './KnittingSymbol.svelte';
  
  export let mode: string;
  export let panelSize: number;
  export let increases: string[];
  export let decreases: string[];
  
  // Grid dimensions
  const rows = Math.random() < 0.5 ? 6 : 8;
  const cols = panelSize;
  const numIncDecPairs = Math.floor(Math.random() * (Math.floor((panelSize - 6) / 2) - 3 + 1)) + 3;
  let patternArrray: string[] = [];
  let knitArray: string[] = [];
let patternRows: string[][] = Array.from({ length: (Math.round(rows/2)) }, () => []);


  function fillPatternArray() {
    console.log(mode);
    if(mode === 'lacy') {
    for (let i = 0; i < numIncDecPairs; i++) {
      patternArrray.push(randomIncrease());
      patternArrray.push(randomDecrease());
    }
    while (patternArrray.length < panelSize) {
      patternArrray.push('knit');
    }
  }
    else {
        for (let i = 0; i < panelSize; i++) {
            patternArrray.push(randomKP());

        }
    
    
}
}

function shuffleArray(array: string[]): string[] {
  const copy = [...array]; // Make a shallow copy
  for (let i = copy.length - 1; i > 0; i--) {
    const j = Math.floor(Math.random() * (i + 1)); // Random index
    [copy[i], copy[j]] = [copy[j], copy[i]]; // Swap elements
  }
  return copy;
}

fillPatternArray();
knitArray = ['knit'
    , ...Array(panelSize - 1).fill('knit')]
console.log('Shuffled Pattern Array:', patternArrray);
for (let i = 0; i < rows; i++) {
  if (i % 2 === 0) {
    // Create a new array copy for even rows
    patternRows[i] = [...knitArray];
  } else {
    // Shuffle a new copy of the pattern array for odd rows
    patternRows[i] = shuffleArray([...patternArrray]);
  }
}

function randomKP() {
    return Math.random() < 0.5 ? 'knit' : 'purl';
}
function randomIncrease() {
    const randomIndex = Math.floor(Math.random() * increases.length);
    return increases[randomIndex]; 
}
function randomDecrease() {
    const randomIndex = Math.floor(Math.random() * decreases.length);
    return decreases[randomIndex]; 
}
function getStitch(row: number, col: number) {
    return patternRows[row][col];
}


</script>

<div style="grid-template-columns: repeat({cols}, 20px); display: grid;">
  {#each Array(rows) as _, row}
    {#each Array(cols) as _, col}
      <KnittingSymbol type={getStitch(row, col)} />
      <!-- <KnittingSymbol type='knit' /> -->
    {/each}
    {/each}

</div>
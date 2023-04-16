<script>
  export let width, svgWidth, svgHeight, quantTicks, colFunc; 
  
</script>

<defs>
    <linearGradient id="mygradient">
        {#each [0, ...quantTicks, 100] as i}
        <stop offset="{i}%" stop-color={colFunc(i)} />
        {/each}
    </linearGradient>
</defs>
<rect
  width={width - 40}
  stroke="gray"
  x="40"
  y={(svgHeight/2) - 50}
  height={svgHeight/4}
  fill="url('#mygradient')"
/>

<script>
  export let width, svgWidth, svgHeight, quantTicks, colScale; 
  
</script>

<defs>
    <linearGradient id="mygradient">
        {#each [0, ...quantTicks, 100] as i}
        <stop offset="{i}%" stop-color={colScale(i)} />
        {/each}
    </linearGradient>
</defs>
<rect
  width={width - 40}
  x="40"
  y={(svgHeight/2) - 50}
  height="50px"
  fill="url('#mygradient')"
/>

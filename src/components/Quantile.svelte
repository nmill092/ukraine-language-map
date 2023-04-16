<script>
    export let svgHeight, svgWidth, quantTicks, colFunc, xScale2; 
</script>

  <g class="quant" stroke="gray">
    
    {#each [0, ...quantTicks] as tick, i} 
    <rect 
                width={xScale2.bandwidth()} 
                fill={colFunc(tick)} 
                height={svgHeight/4}
                y={(svgHeight/2) - 50}
                x={xScale2(tick)}></rect>
    {/each}
    </g>

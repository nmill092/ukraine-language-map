<script>
    export let svgHeight, svgWidth, quantTicks, quantScale, xScale2; 
</script>

  <g class="quant">
    {#each [0, ...quantTicks] as tick, i} 
    <rect 
                width={xScale2.bandwidth()} 
                fill={quantScale(tick)} 
                height="50px" 
                y={(svgHeight/2) - 50}
                x={xScale2(tick)}></rect>
    {/each}
    </g>

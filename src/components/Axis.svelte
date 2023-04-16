<script>
    import { fade } from 'svelte/transition'
    export let svgWidth, svgHeight, xScale, quantTicks; 
</script>

    <g class="axis">
    {#each [0, ...quantTicks, 100] as i}
        <g class="tick" in:fade>
        <line
            x1={xScale(i)}
            x2={xScale(i)}
            y1={-(svgHeight/2)-20}
            y2={-((svgHeight/2) + 50)}
            stroke-width={i === 0 ? 0 : 1}
            stroke-dasharray="1 1"
            stroke="none"
            transform="translate(0, 170)"
        />
        <text text-anchor="middle" 
                x={xScale(i)} y={(svgHeight/2) + 10}> 
        {Math.floor(i)}%
        </text>
        </g>
    {/each}
    </g>

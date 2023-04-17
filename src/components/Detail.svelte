<script>
    import {scaleLinear, scaleBand} from 'd3';
    export let stat, clientWidth, selectedRegion, colFunc; 

    $: xScale = scaleLinear().domain([0, 100]).range([20, clientWidth-100]);
    $: yScale = scaleBand().domain(["Ukrainian", "Russian"]).range([120, 0]).padding(.5)

</script>

{#if selectedRegion}
{#if stat === "ukrl1" || stat === "rusl1"}
<div class="chart">
    <svg height="50%" width="100%">
        <g class="y-axis">
            {#each yScale.domain() as i}
                <text dx={10} 
                      dy={-5} 
                      x="10" 
                      y={yScale(i)}>{i}</text>
            {/each}
        </g>
        <g class="x-axis">
            {#each xScale.ticks(3) as tick}
                <line 
                    x1={xScale(tick)+5}
                    x2={xScale(tick)+5}
                    y1={-107}
                    y2={100}
                    stroke="gray" 
                    stroke-dasharray="1 2"
                    opacity=".5"
                    stroke-width="0.8">

                </line>
                <text 
                    text-anchor="middle"
                    x={xScale(tick)+5} 
                    y={125}>
                    {tick}%
                </text>
            {/each}
        </g>
        <g class="l1-chart">
            <g>
            <rect height={yScale.bandwidth()} 
                width={xScale(selectedRegion.rusl1)} 
                stroke="slategray"
                  x={20}
                  y={yScale("Russian")}
                  fill={colFunc(selectedRegion.rusl1)}></rect>
            <text
                class="lbl"
                x={(xScale(selectedRegion.rusl1) + 30)}
                y={yScale("Russian") + yScale.bandwidth()/2 + 5}>
                {Math.ceil(selectedRegion.rusl1)}%
            </text>
                </g>
                <g>
            <rect 
                  height={yScale.bandwidth()} 
                  width={xScale(selectedRegion.ukrl1)}
                  x={20}
                  y={yScale("Ukrainian")}
                  stroke="slategray"
                  fill={colFunc(selectedRegion.ukrl1)}></rect>
            <text
                  class="lbl"
                  x={(xScale(selectedRegion.ukrl1) + 30)}
                  y={yScale("Ukrainian") + (yScale.bandwidth()/2) + 5}>
                  {Math.ceil(selectedRegion.ukrl1)}%
              </text>
                </g>
        </g>
     
    </svg>
</div>
{/if}
{#if stat==="rus" || stat==="ukr"}
<div class="chart">
    <svg height="50%" width="100%">
        <g class="y-axis">
            {#each yScale.domain() as i}
                <text dx={10} 
                      dy={-5} 
                      x="10" 
                      y={yScale(i)}>{i}</text>
            {/each}
        </g>
        <g class="x-axis">
            {#each xScale.ticks(3) as tick}
                <line 
                    x1={xScale(tick)+5}
                    x2={xScale(tick)+5}
                    y1={-107}
                    y2={100}
                    stroke="gray" 
                    stroke-dasharray="1 2"
                    opacity=".5"
                    stroke-width="0.8">

                </line>
                <text 
                    text-anchor="middle"
                    x={xScale(tick)+5} 
                    y={125}>
                    {tick}%
                </text>
            {/each}
        </g>
        <g class="l1-chart">
            <g>
            <rect height={yScale.bandwidth()} 
                width={xScale(selectedRegion.rus)} 
                  x={20}
                  y={yScale("Russian")}
                  stroke="slategray"
                  fill={colFunc(selectedRegion.rus)}></rect>
            <text
                class="lbl"
                x={(xScale(selectedRegion.rus) + 30)}
                y={yScale("Russian") + yScale.bandwidth()/2 + 5}>
                {Math.ceil(selectedRegion.rus)}%
            </text>
                </g>
                <g>
            <rect 
                  height={yScale.bandwidth()} 
                  width={xScale(selectedRegion.ukr)}
                  x={20}
                  y={yScale("Ukrainian")}
                  stroke="slategray"
                  fill={colFunc(selectedRegion.ukr)}></rect>
            <text
                  class="lbl"
                  x={(xScale(selectedRegion.ukr) + 30)}
                  y={yScale("Ukrainian") + (yScale.bandwidth()/2) + 5}>
                  {Math.ceil(selectedRegion.ukr)}%
              </text>
                </g>
        </g>
     
    </svg>
</div>
{/if}
{/if}

<style>
    .chart {
        margin-left: auto;
        margin-right: auto;
        margin-top: 1.5rem; 
    }
    .lbl {
        font-weight: 500;
        transition: .2s ease all; 
    }
</style>
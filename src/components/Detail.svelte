<script>
    import * as d3 from 'd3';
    import { tweened } from 'svelte/motion';
    import { onMount } from 'svelte';
    import { quadInOut } from 'svelte/easing';

    export let clientWidth, selectedRegion, colFunc; 
    
    onMount(() => {
        tweenedWidth1.set(xScale(selectedRegion.rusl1))
        tweenedWidth2.set(xScale(selectedRegion.ukrl1))
        tweenedText1.set(selectedRegion.rusl1)
        tweenedText2.set(selectedRegion.ukrl1)

        tweenedWidth3.set(xScale(selectedRegion.rus))
        tweenedWidth4.set(xScale(selectedRegion.ukr))
        tweenedText3.set(selectedRegion.rus)
        tweenedText4.set(selectedRegion.ukr)
    })

    const tweenedWidth1 = tweened(0, { duration: 700, easing: quadInOut }); 
    const tweenedWidth2 = tweened(0, { duration: 900, easing: quadInOut }); 
    const tweenedWidth3 = tweened(0, { duration: 700, easing: quadInOut }); 
    const tweenedWidth4 = tweened(0, { duration: 900, easing: quadInOut }); 
    
    
    const tweenedText1 = tweened(0, {duration: 700});
    const tweenedText2 = tweened(0, {duration: 900});
    const tweenedText3 = tweened(0, {duration: 700});
    const tweenedText4 = tweened(0, {duration: 900});

    $: xScale = d3.scaleLinear().domain([0, 100]).range([20, clientWidth-100]);
    $: yScale = d3.scaleBand().domain(["Ukrainian", "Russian"]).range([120, 0]).padding(.5)

</script>

{#if selectedRegion}
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
                width={$tweenedWidth1} 
                  x={20}
                  y={yScale("Russian")}
                  fill={colFunc(selectedRegion.rusl1)}></rect>
            <text
                class="lbl"
                x={($tweenedWidth1 + 30)}
                y={yScale("Russian") + yScale.bandwidth()/2 + 5}>
                {Math.ceil($tweenedText1)}%
            </text>
                </g>
                <g>
            <rect 
                  height={yScale.bandwidth()} 
                  width={$tweenedWidth2}
                  x={20}
                  y={yScale("Ukrainian")}
                  fill={colFunc(selectedRegion.ukrl1)}></rect>
            <text
                  class="lbl"
                  x={($tweenedWidth2 + 30)}
                  y={yScale("Ukrainian") + (yScale.bandwidth()/2) + 5}>
                  {Math.ceil($tweenedText2)}%
              </text>
                </g>
        </g>
     
    </svg>
</div>

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
                width={$tweenedWidth3} 
                  x={20}
                  y={yScale("Russian")}
                  fill={colFunc(selectedRegion.rus)}></rect>
            <text
                class="lbl"
                x={($tweenedWidth3 + 30)}
                y={yScale("Russian") + yScale.bandwidth()/2 + 5}>
                {Math.ceil($tweenedText3)}%
            </text>
                </g>
                <g>
            <rect 
                  height={yScale.bandwidth()} 
                  width={$tweenedWidth4}
                  x={20}
                  y={yScale("Ukrainian")}
                  fill={colFunc(selectedRegion.ukr)}></rect>
            <text
                  class="lbl"
                  x={($tweenedWidth4 + 30)}
                  y={yScale("Ukrainian") + (yScale.bandwidth()/2) + 5}>
                  {Math.ceil($tweenedText4)}%
              </text>
                </g>
        </g>
     
    </svg>
</div>
{/if}

<style>
    .chart {
        margin-top: 1.5rem; 
    }
    .lbl {
        font-weight: 500;
        transition: .2s ease all; 
    }
</style>
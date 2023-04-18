<script>
  import Axis from "./Axis.svelte";
  import Linear from "./Linear.svelte";
  import Quantile from "./Quantile.svelte";
  import GuideLine from "./GuideLine.svelte";
  import {
    scaleBand,
    scaleLinear,
    scaleSequential,
    scaleQuantile,
    quantize, 
    interpolateRdYlBu  
} from "d3";

  export let guideLinePos, 
            colFunc,
            size,
            stat,
            strategy="colScale",
            interpolator = interpolateRdYlBu; 

  let svgWidth = 500, svgHeight = 150, width =  svgWidth -40;

  $: xScale = scaleLinear().domain([0, 100]).range([40, svgWidth -40]);

  $: colScale = scaleSequential(interpolator).domain([0, 100]);

  $: quantScale = scaleQuantile(quantize(interpolator, size)).domain([0, 100]);
  $: quantTicks = quantScale.quantiles();

  $: xScale2 = scaleBand().domain([0, ...quantTicks]).range([40,  svgWidth -40]);
  $: colFunc = strategy === "quantScale" ? quantScale : colScale;

</script>

<div class="legend-container"> 
 
  <span class="legend-title">
    {#if stat==="ukrl1"}
    % of Native (L1) Ukrainian Speakers
    {:else if stat==="rusl1"}
    % of Native (L1) Russian Speakers
    {:else if stat==="rus"}
    % of Population Proficient in Russian
    {:else if stat==="ukr"}
    % of Population Proficient in Ukrainian
    {/if}
    
  </span>

  <svg class="legend" width={svgWidth} height="100px">
    {#if strategy === "colScale"}
    <Linear {width} {svgHeight} {quantTicks} {colFunc}/>
    {/if}
    {#if strategy === "quantScale"}
     <Quantile {svgHeight} {svgWidth} {xScale2} {quantTicks} {colFunc} />
     {/if} 
     <GuideLine {xScale} {guideLinePos}/>

    <Axis {xScale} {quantTicks} {svgHeight} {svgWidth}/>
</svg>
</div>

<style>
.legend-container {
    box-shadow: 10px 10px 10px rgba(0,0,0,0.1);
    display: flex;
    padding: 1rem;
    flex-direction: column;
    background-color: rgba(255,255,255,0.8);
    border-radius: 10px; 
    position: absolute; 
    bottom: 5%; 
    right: 5%;
    z-index: 5000;
}

.legend-title {
    font-family: var(--main-font);
    font-weight: 600; 
    font-size: 1.2rem; 
}
</style>

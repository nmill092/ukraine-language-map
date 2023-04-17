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
            strategy="colScale",
            interpolator = interpolateRdYlBu; 


  let svgWidth = 500, svgHeight = 150, width = svgWidth/1.2;



  $: xScale = scaleLinear().domain([0, 100]).range([40, svgWidth / 1.2]);

  $: colScale = scaleSequential(interpolator).domain([0, 100]);

  $: quantScale = scaleQuantile(quantize(interpolator, size)).domain([0, 100]);
  $: quantTicks = quantScale.quantiles();

  $: xScale2 = scaleBand().domain([0, ...quantTicks]).range([40, svgWidth / 1.2]);
  $: colFunc = strategy === "quantScale" ? quantScale : colScale;

</script>

<div class="legend-container">
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
    position: absolute; 
    bottom: 10%; 
    right: 0%;
    z-index: 5000;
}
</style>

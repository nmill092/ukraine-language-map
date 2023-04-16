<script>
  import * as d3 from "d3";
  import { fly, fade } from 'svelte/transition';
  import Axis from "./Axis.svelte";
  import Linear from "./Linear.svelte";
  import Quantile from "./Quantile.svelte";
  import GuideLine from "./GuideLine.svelte";
  export let size, 
            guideLinePos, 
            interpolator, 
            quantScale, 
            colFunc, 
            colScale, 
            strategy="quantScale"; 
  
  let svgWidth = 900, svgHeight = 150, width = svgWidth/2;

  $: xScale = d3.scaleLinear().domain([0, 100]).range([40, width]);
  $: colScale = d3.scaleSequential(interpolator).domain([0, 100]);
  $: quantScale = d3
    .scaleQuantile(d3.quantize(interpolator, size))
    .domain([0, 100]);
  $: quantTicks = quantScale.quantiles();
  $: xScale2 = d3
    .scaleBand()
    .domain([0, ...quantTicks])
    .range([40, width]);
  $: colFunc = strategy === "quantScale" ? quantScale : colScale;

</script>

  <svg class="legend" viewBox="0 0 {svgWidth} {svgHeight}">
    {#if strategy === "colScale"}
    <Linear {width} {svgHeight} {svgWidth} {quantTicks} {colFunc}/>
    {/if}
    {#if strategy === "quantScale"}
     <Quantile {svgHeight} {svgWidth} {xScale2} {quantTicks} {colFunc} />
     {/if} 
     <GuideLine {xScale} {guideLinePos}/>

    <Axis {xScale} {quantTicks} {svgHeight} {svgWidth}/>
</svg>

<style>
    .legend {
        position: absolute;
        bottom: 0;
    }
</style>

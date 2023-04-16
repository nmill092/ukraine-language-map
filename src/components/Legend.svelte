<script>
  import * as d3 from "d3";
  import { fly, fade } from 'svelte/transition';
  import Axis from "./Axis.svelte";
  import Linear from "./Linear.svelte";
  import Quantile from "./Quantile.svelte";
  export let size, interpolator, quantScale, colScale, strategy; 
  let width = 790;
  let svgWidth = 900; 
  let svgHeight = 150; 

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
</script>

  <svg in:fly={{duration:5000}} height={svgHeight} width={svgWidth}>
    {#if strategy === "colScale"}
    <Linear {width} {svgHeight} {svgWidth} {quantTicks} {colScale}/>
    {/if}
    {#if strategy === "quantScale"}
     <Quantile {svgHeight} {svgWidth} {xScale2} {quantTicks} {quantScale} />
     {/if} 
    <Axis {xScale} {quantTicks} {svgHeight} {svgWidth}/>
</svg>



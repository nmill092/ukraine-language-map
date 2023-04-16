<script>
  import * as d3 from "d3";
  import { onMount, createEventDispatcher } from "svelte";
  import { draw, fade } from 'svelte/transition';
  import namesMap from "../namesMap";
  import Tooltip from "./Tooltip.svelte";
  export let colFunc, strategy="quantScale", hovering;
  let geoData, langData, worldData, tooltipPos, selectedRegion, percentage; 
  let screenSize, screenHeight; 

  const dispatch = createEventDispatcher();

  function updateGuideLine(event, hovering, perc, hovered) {
    selectedRegion = hovered; 
    percentage = perc; 
    let {clientX: tooltipX, clientY: tooltipY} = event;
    tooltipPos = {x: tooltipX, y: tooltipY}; 
 
    dispatch("guideline", {
      perc: perc,
      hovering: hovering,
      hovered: hovered
    });
  }

  onMount(() => {
    d3.json("world.json").then((d) => {
        worldData = d; 
    })

    d3.json("UA_FULL_Ukraine.geojson").then((d) => {
      geoData = d;
    });

    d3.csv("ua_lang_admin1_v02.csv", (d) => {
      return {
        region: namesMap[d.admin1_name],
        numLangSpoke: +d.number_of_named_languages,
        mainLang: d.main_language,
        ukrl1: +d["Ukrainian L1"] * 100,
        rusl1: +d["Russian L1"] * 100,
        ukr: +d.Ukrainian * 100,
        rus: +d.Russian * 100,
        pop: +d.pop_total,
      };
    }).then((d) => {
      langData = d;
    });
  });

  let projection = d3.geoMercator().scale(2300).translate([-650, 2550]);
  let geoPath = d3.geoPath(projection);
</script>
<svelte:window bind:innerWidth={screenSize} bind:innerHeight={screenHeight}/>

<div class="map-container">
    <Tooltip {tooltipPos} {selectedRegion} {hovering} {colFunc} {percentage} />
<svg viewBox="0 0 1200 {screenHeight}">
  {#if geoData && langData && worldData && colFunc}
  <g>
    {#each worldData.features as feature}
    <path in:fade d={geoPath(feature)} fill="ghostwhite" stroke="gray" opacity=".3"></path>
    {/each}
  </g>
    <g in:fade={{delay:800}}>
      {#each geoData.features as feature, i}
        <path
          on:focus={(event) => updateGuideLine(event, true, langData.find((i) => i.region === feature.properties["name:en"]).ukrl1, langData.find((i) => i.region === feature.properties["name:en"]))}
          on:mousemove={(event) => updateGuideLine(event, true, langData.find((i) => i.region === feature.properties["name:en"]).ukrl1, langData.find((i) => i.region === feature.properties["name:en"]))}
          on:mouseleave={(event) => updateGuideLine(event, false, null, null)}
          d={geoPath(feature)}
          fill={
          colFunc(
            langData.find((i) => i.region === feature.properties["name:en"]).ukrl1
          )}
          opacity={selectedRegion && feature.properties["name:en"]===selectedRegion.region ? 1 : !selectedRegion ? 1 : .5}
          stroke="#000"
          stroke-width="1"
        />
      {/each}
    </g>
  {/if}
</svg>
</div>
<style>
  * {
    transition: 0.5s ease all;
  }
  .map-container {
    position: relative; 
  }
</style>

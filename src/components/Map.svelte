<script>
  import { zoom, json, csv, select, geoMercator, geoPath } from 'd3';
  import { onMount, createEventDispatcher } from "svelte";
  import { fade } from "svelte/transition";
  import namesMap from "../namesMap";
  import Tooltip from "./Tooltip.svelte";
  export let colFunc, hovering, stat;
  let geoData,
    langData,
    maps,
    worldData,
    tooltipPos,
    selectedRegion,
    percentage;
    
  const dispatch = createEventDispatcher();

  function updateGuideLine(event, hovering, perc, hovered) {
    selectedRegion = hovered;
    percentage = perc;
    let { clientX: tooltipX, clientY: tooltipY } = event;
    tooltipPos = { x: tooltipX, y: tooltipY };

    dispatch("guideline", {
      perc: perc,
      hovering: hovering,
      hovered: hovered,
    });
  }

  onMount(() => {
    json("world.json").then((d) => {
      worldData = d;
    });

    json("UA_FULL_Ukraine.geojson").then((d) => {
      geoData = d;
    });

    csv("ua_lang_admin1_v02.csv", (d) => {
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

    select(maps).call(zoom().scaleExtent([1, 1]).on("zoom", handleZoom));
  });

  function handleZoom(e) {
    select(maps).select(".map").attr("transform", e.transform);
  }

  let projection = geoMercator().scale(2100).translate([-520, 2300]);
  let _geoPath = geoPath(projection);
</script>

<div bind:this={maps}>
  <Tooltip {stat} {tooltipPos} {selectedRegion} {hovering} {colFunc} />
  <svg viewBox="0 0 1200 1200">
    {#if geoData && langData && worldData && colFunc}
      <g class="map">
        <g class="worldmap">
          {#each worldData.features as feature}
            <path
              in:fade={{ duration: 1000, delay: 1000 }}
              d={_geoPath(feature)}
              fill="slategray"
              stroke="gray"
              opacity=".3"
            />
          {/each}
        </g>
        <g in:fade={{ duration: 1000 }} class="ukrmap">
          {#each geoData.features as feature, i}
            <path
              on:focus={(event) =>
                updateGuideLine(
                  event,
                  true,
                  langData.find(
                    (i) => i.region === feature.properties["name:en"]
                  )[stat],
                  langData.find(
                    (i) => i.region === feature.properties["name:en"]
                  )
                )}
              on:mouseenter={(event) =>
                updateGuideLine(
                  event,
                  true,
                  langData.find(
                    (i) => i.region === feature.properties["name:en"]
                  )[stat],
                  langData.find(
                    (i) => i.region === feature.properties["name:en"]
                  )
                )}
              on:mouseleave={(event) =>
                updateGuideLine(event, false, null, null)}
              d={_geoPath(feature)}
              fill={colFunc(
                langData.find(
                  (i) => i.region === feature.properties["name:en"]
                )[stat]
              )}
              opacity={selectedRegion &&
              feature.properties["name:en"] === selectedRegion.region
                ? 1
                : !selectedRegion
                ? 1
                : 0.5}
              stroke="slategray"
              stroke-width="1"
            />
          {/each}
        </g>
      </g>
    {/if}
  </svg>
</div>

<style>
  path:focus {
    outline: none; 
  }
 path {
  transition: .2s ease all;
 }
</style>

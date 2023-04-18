<script>
  import { zoom, json, csv, select, geoMercator, geoPath } from 'd3';
  import { onMount, createEventDispatcher } from "svelte";
  import { fade, fly } from "svelte/transition";
  import namesMap from "../namesMap";
  import { easeCubicInOut } from 'd3';
  import { cities, countries } from '../data/cities';
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

    select(maps).call(zoom().scaleExtent([1, 1.3]).on("zoom", handleZoom));
  });

  function handleZoom(e) {
    select(maps).select(".map").attr("transform", e.transform);
  }

  let projection = geoMercator().scale(1800).translate([-200, 2000]);
  let _geoPath = geoPath(projection);
  let innerHeight,innerWidth;
</script>
<svelte:window bind:innerWidth={innerWidth} bind:innerHeight={innerHeight}></svelte:window>
<div bind:this={maps}>
  <Tooltip {stat} {tooltipPos} {selectedRegion} {hovering} {colFunc} />
  <svg style="background-color:#BCE3F2" viewBox="0 0 1280 5000">
    {#if geoData && langData && worldData && colFunc}
      <g class="map">
        <g in:fade={{delay: 500}} class="worldmap">
          {#each worldData.features as feature}
            <path
              d={_geoPath(feature)}
              fill="#F8F0E3"
              stroke="slategray"
              stroke-width="0.5"
              opacity="1"
            />
          {/each}
        </g>
    
        <g in:fade={{ duration: 1000, delay: 1000 }} class="ukrmap">
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
              on:mouseover={(event) =>
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
              stroke={selectedRegion &&
                feature.properties["name:en"] === selectedRegion.region
                  ? "black"
                  : !selectedRegion
                  ? "#E5E4E2"
                  : "#E5E4E2"}
              stroke-width="1"
            />
          {/each}
        </g>
        <g  class="cities">
          {#each cities.features as city, i}
          <g in:fly={{delay: 2000 + (i*30), y: -200, easing:easeCubicInOut }} class="city">
            <text  text-anchor="middle" stroke="none" stroke-width=".1" dy="-10" class="city-name" fill="white" transform="translate({projection(city.geometry.coordinates)})">{city.properties.city}</text>
            <circle r="4" transform="translate({projection(city.geometry.coordinates)})" fill="white" stroke="slategray">
            </circle>

          </g>
          {/each}
        </g>
        <g in:fly={{delay: 2000}} class="countries">
          {#each countries.features as country, i}
          <g class="country">
            <text text-anchor="middle" 
                  font-size="{country.properties.country === "Moldova" || country.properties.country === "Sea of Azov" ? '.7rem' : '.85rem'}"
                  dy="-10" 
                  class="country-name" 
                  transform="translate({projection(country.geometry.coordinates)}) {country.properties.country === "Moldova" ? 'rotate(70)' : ''}">{country.properties.country}</text>
          </g>
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
 .city {
  pointer-events: none; 
 }
 .city-name {
  text-shadow: 5px 5px 20px black, -5px -5px 20px white;
  font-size: .7rem; 
  font-family: "Fira Sans";
  font-weight: 600;
 }

 .country-name {
  font-family: "Fira Sans";
  fill: slategray;
  font-weight: 800;
  text-transform: uppercase;
 }
</style>

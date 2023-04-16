<script>
  import * as d3 from "d3";
  import { onMount, createEventDispatcher } from "svelte";
  import namesMap from "../namesMap";

  export let quantScale, colScale, strategy;
  let geoData, langData;

  $: colFunc = strategy === "quantScale" ? quantScale : colScale;

  const dispatch = createEventDispatcher();

  function updateGuideLine(e) {
    dispatch("guideline", {
      x: e,
    });
  }

  onMount(() => {
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

  let projection = d3.geoMercator().scale(2100).translate([-750, 2300]);
  let geoPath = d3.geoPath(projection);
</script>

<svg viewBox="0 0 800 500">
  {#if geoData && langData && quantScale && colScale}
    <g>
      {#each geoData.features as feature}
        <path
          on:focus={() => updateGuideLine(langData.find((i) => i.region === feature.properties["name:en"]).ukrl1)}
          on:mouseover={() => updateGuideLine(langData.find((i) => i.region === feature.properties["name:en"]).ukrl1)}
          on:mouseleave={() => updateGuideLine(0)}
          d={geoPath(feature)}
          fill={
          colFunc(
            langData.find((i) => i.region === feature.properties["name:en"]).ukrl1
          )}
          stroke="#eaeaea"
          stroke-width="1"
        />
      {/each}
    </g>
  {/if}
</svg>

<style>
  * {
    transition: 0.5s ease all;
  }
</style>

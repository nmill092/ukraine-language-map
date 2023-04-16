<script>
  import * as d3 from "d3";
  import { onMount } from "svelte";
  import namesMap from "../namesMap";

  export let interpolator; 
  export let colors; 
  export let quantScale; 
  export let colScale; 
  export let strategy; 

  
  let geoData;
  let langData;

  $: colFunc = strategy === "quantScale" ? quantScale : colScale; 




  onMount(() => {
    d3.json("UA_FULL_Ukraine.geojson").then((d) => {
      geoData = d
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

  let projection = d3.geoMercator().scale(2200).translate([-750, 2450]);
  let geoPath = d3.geoPath(projection);
</script>

<svg height="600" width="900">
  {#if geoData && langData && quantScale && colScale}
    <g>
      {#each geoData.features as feature}
        <path d={geoPath(feature)} fill={colFunc(langData.find(i => i.region === feature.properties["name:en"]).ukr)} stroke="#eaeaea" stroke-width="1" />
        <title>{feature.properties["name:en"]}, {langData.find(i => i.region === feature.properties["name:en"]).ukr1}</title>
      {/each}
    </g>
  {/if}
</svg>

<!-- {#if geoData && langData}
<ol>
 {#each geoData.features as feature}
<li>{langData.find(i => i.region === feature.properties["name:en"]).rusl1} - {colScale(langData.find(i => i.region === feature.properties["name:en"]).rusl1)}</li> 
 {/each}
</ol>
{/if} -->
<style>
    * { 
        transition: .5s ease all; 
    }
</style>
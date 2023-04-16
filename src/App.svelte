<script>
  import Legend from "./components/Legend.svelte";
  import Map from "./components/Map.svelte";
  import * as d3chromatic from "d3-scale-chromatic";
  import * as d3 from "d3";
  let size = 4, quantScale, guideLinePos=0, hovering, hovered, tooltipPos, colFunc, strategy, interpolator = d3.interpolateRdYlBu;
  $: interpolators = Object.entries(d3chromatic).filter(([key, value]) =>
    key.startsWith("interpolate")
  );

  function updateGuideLinePos(e) {
    guideLinePos = e.detail.perc; 
    hovering = e.detail.hovering; 
    hovered = e.detail.hovered; 
  }
</script>

<!-- <Map/> -->

<div class="container">
  <Map 
      on:guideline={updateGuideLinePos} 
      {strategy} 
      {colFunc}
      {hovering}/>

  <input type="number" min="2" max="15" bind:value={size} />
  {size} 

  <Legend {strategy} {guideLinePos} bind:colFunc={colFunc} bind:quantScale={quantScale} {interpolator} {size}/>
  
 <select bind:value={strategy}>
  <option value="quantScale">quantScale</option>
  <option value="colScale">colScale</option>
</select>
  <select bind:value={interpolator}>
    {#each interpolators as i}
      <option value={i[1]}>{i[0].replace("interpolate", "")}</option>
    {/each}
  </select>
</div>
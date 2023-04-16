<script>
  import Legend from "./components/Legend.svelte";
  import Map from "./components/Map.svelte";
  import * as d3chromatic from "d3-scale-chromatic";
  import * as d3 from "d3";
  let size = 4;
  let quantScale; 
  let colScale; 
  let colors; 
  let strategy; 
  let interpolator = d3.interpolateTurbo;
  $: interpolators = Object.entries(d3chromatic).filter(([key, value]) =>
    key.startsWith("interpolate")
  );
</script>

<!-- <Map/> -->

<div class="container">
  <Map {interpolator} {quantScale} {strategy} {colScale} {colors}/>

  <input type="number" min="2" max="15" bind:value={size} />
  {size} 

  <Legend bind:colors={colors} {strategy} bind:colScale={colScale} bind:quantScale={quantScale} {interpolator} {size}/>
  
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

<style>
    @import url("https://fonts.googleapis.com/css2?family=Chivo+Mono:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;1,100;1,200;1,400;1,500&family=Noto+Sans:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;1,100;1,200;1,300;1,400;1,500&display=swap");

  .container {
    background-color: ghostwhite;
    height: 100vh;
  }


  .axis text {
    font-family: "Chivo Mono";
    font-weight: 100;
    font-size: 0.8rem;
  }

  rect {
    transition: 0.8s ease fill, 0.8s ease opacity;
    opacity: 1;
  }

  rect:hover {
    opacity: 0.8;
    z-index: 400;
  }
</style>

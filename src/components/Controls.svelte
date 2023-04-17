<script>
  import * as d3chromatic from "d3-scale-chromatic";

  export let strategy = "colScale",
    stat = "ukrl1",
    size = 5,
    interpolator; 

  let interpolators = Object.entries(d3chromatic).filter(([key, value]) =>
    key.startsWith("interpolate")
  );

</script>

<div class="bottom-matter">
<div class="description">
<h2>Ukraine Language Map</h2>
</div>
<div class="controls">

  

  <input type="number" min="2" max="15" bind:value={size} />

  <select bind:value={strategy}>
    <option value="quantScale">quantScale</option>
    <option value="colScale">colScale</option>
  </select>

  <select bind:value={interpolator}>
    {#each interpolators as i}
      <option value={i[1]}>{i[0].replace("interpolate", "")}</option>
    {/each}
  </select>

  <select bind:value={stat}>
    <option value="ukrl1">% Ukrainian as a First Language</option>
    <option value="rusl1">% Russian as a First Language</option>
    <option value="ukr">% Population Proficient in Ukrainian</option>
    <option value="rus">% Population Proficient in Russian</option>
  </select>
</div>
</div>
<style>
  .bottom-matter { 
    background-color: gainsboro;
    height: 100%; 
  }
  .description {
    font-family: 'Noto Serif Hebrew';
  }
  .controls {
    display: flex;
    padding: 5px 10px;
    z-index: 6000;
    width: 50%;
   
  }
</style>

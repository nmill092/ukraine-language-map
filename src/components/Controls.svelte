<script>
  import { fly, fade } from 'svelte/transition'
  import * as d3chromatic from "d3-scale-chromatic";

  export let strategy = "colScale",
    stat = "ukrl1",
    size = 5,
    interpolator; 

  let interpolators = Object.entries(d3chromatic).filter(([key, value]) =>
    key.startsWith("interpolate")
  );

</script>


<div in:fade={{duration:2000}} class="description">
<h2>How Ukrainian and Russian </h2>
<p> 
  <strong>Language in Ukraine is a deeply political issue.</strong> 
  While many analyses divide the country into a Ukrainian-speaking west and a Russian-speaking east, the reality is more nuanced.</p>
<p> This map depicts data on language affiliation and proficiency in each region of Ukraine.* 
  Use the selection box below to control which statistic is visualized on the map. 
  You can also change the color scheme and customize the scale used to categorize the data.
</p> 
</div>

<div class="controls">

  <div class="control-row">
    <label>Select a statistic to display: 
    <select bind:value={stat}>
      <option value="ukrl1">% Ukrainian as a First Language</option>
      <option value="rusl1">% Russian as a First Language</option>
      <option value="ukr">% Population Proficient in Ukrainian</option>
      <option value="rus">% Population Proficient in Russian</option>
    </select>
  </label>
  </div>

<div class="control-row">
  <label> Select a color scale type** 
  <select bind:value={strategy}>
    <option value="quantScale">Discrete</option>
    <option value="colScale">Continuous</option>
  </select>
  </label>

  <span class="note">** Continuous scales help with perceiving subtle differences between regions, while discrete scales help visualize patterns and clusters.</span>

</div>


<div class="control-row">
  <label> Choose a number of 
    <input type="number" min="2" max="15" bind:value={size} />
  </label>
</div>


  <div class="control-row">
    <label> Select a color scheme
    <select bind:value={interpolator}>
      {#each interpolators as i}
        <option value={i[1]}>{i[0].replace("interpolate", "")}</option>
      {/each}
    </select>
  </label>
  </div>
</div>

<div class="sidebar-footer">
<span class="note">* Data are based on Ukraine's 2001 census, the country's only nationwide census to date. Demographic and political shifts, from the 2004 Orange Revolution to Russia's invasion of Ukraine in 2022, may have affected language affiliation in the country.</span>
<span class="note">* Significant minority languages not depicted on this map include Hungarian, Crimean Tatar, Romanian, Greek, Bulgarian, Polish, Rusyn, and Romani, among others.</span>
</div>

<style>
  .description {
    margin-top: 2rem;
    font-weight: 300;
    font-family: var(--main-font);
    flex: 1 0 25%; 
  }
  .description h2, p {
    margin-bottom: 1rem;
  }
  .controls {
    padding: 5px 10px;
    z-index: 6000;
    flex: 1 0 40%; 
    gap: 1rem;
    display: flex; 
    flex-direction: column;
  }

  .controls select {
    border: 1px solid slategray; 
    border-radius: 10px; 
    font-weight: 400;
    font-size: 1rem;
    width: 100%; 
    padding: .5rem 1.2rem; 
    font-family: var(--main-font); 
  }

  .controls input[type="number"] {
    border: 1px solid slategray; 
    border-radius: 10%; 
    padding: .5rem 1.2rem; 
    font-family: var(--main-font);
    font-size: 1rem; 
  }

  .controls label { 
    font-weight: 600; 
  }

  .note {
    display: block; 
    margin: .5rem 0; 
    font-style: italic; 
    font-weight: 100;
    font-size: .9rem;
    flex: 1 0 35%;  
  }
</style>

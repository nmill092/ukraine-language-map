<script>
  import Legend from "./components/Legend.svelte";
  import Map from "./components/Map.svelte";
  import Controls from "./components/Controls.svelte";

  let size, 
      guideLinePos, 
      stat, 
      hovering, 
      hovered, 
      colFunc, 
      strategy, 
      interpolator;
  

  function updateGuideLinePos(e) {
    guideLinePos = e.detail.perc; 
    hovering = e.detail.hovering; 
    hovered = e.detail.hovered; 
  }
</script>


<div class="container">
 <div class="header">
    <Controls 
    bind:strategy = {strategy} 
    bind:stat={stat}
    bind:size={size} 
    bind:interpolator = {interpolator}/>
</div>
  <div class="map-container">
      <Map 
          on:guideline={updateGuideLinePos} 
          {colFunc}
          {hovering}
          {stat}/>

          <Legend {strategy} 
          {guideLinePos} 
          {size}
          {stat}
          {interpolator}
          bind:colFunc={colFunc} />
  </div>



  


     
</div>
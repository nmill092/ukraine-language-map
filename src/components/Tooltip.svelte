<script>
import { fly } from 'svelte/transition';
import Detail from './Detail.svelte';
export let tooltipPos, stat, hovering, selectedRegion, colFunc; 
let clientWidth; 

</script>

{#if hovering && selectedRegion}
<div bind:clientWidth={clientWidth} class="tooltip"  style="top: {tooltipPos.y}px; left: {tooltipPos.x}px">
    <div class="tooltip-bar">
        <div class="tooltip-bar__left"></div>
        <div class="tooltip-bar__right"></div>
    </div>   
    <span class="tooltip-header">{selectedRegion.region}</span>
    <div class="tooltip-content">
       
    <div class="tooltip-row">
        {#if stat === "ukr" || stat === "rus"}
        <span class="tooltip-question">
            What percentage of residents are<br/> proficient in Russian or Ukrainian?
        </span>
        {:else}
        <span class="tooltip-question">
            What percentage of residents speak Russian or Ukrainian <strong>as a first language (L1)?</strong>
        </span>
        {/if}
    </div>
   
    <Detail {stat} {clientWidth} {colFunc} {selectedRegion}/>
</div>
</div>
{/if}

<style>
   .tooltip {
    border-radius: 5px;
    transition: .5s ease all; 
    pointer-events: none; 
    overflow:hidden;
    z-index: 6000;
    padding: 8px 11px;
    position: absolute; 
    min-width: 250px; 
    max-width: 300px; 
    background-color: rgba(255,255,255,.9); 
    box-shadow: rgba(0, 0, 0, 0.15) 2px 3px 8px;
   }
   .tooltip-row {
    display: flex; 
    margin-bottom: .3rem; 
    justify-content: space-around;
   }

   .tooltip-question {
    font-family: "Noto Serif Hebrew";
    display: block;
   }
   .tooltip-content {
    margin-top: 1rem; 
    font-family: "Noto Serif Hebrew";
    font-weight: 300;
   }
  

   .tooltip-header { 
    display: block;
    font-size: 1.7rem;
    margin-top: 1rem; 
    font-family: "Noto Serif Hebrew";
    font-weight: 400;
   }
   .tooltip-bar {
    position: absolute;
    width: 100%; 
    height: 14px;
    top: 0;
    left: 0; 
    display: flex; 
   }

   .tooltip-bar__left {
    height: 100%; 
    width: 50%; 
    background-color: #0057b7;
   }

   .tooltip-bar__right {
    height: 100%; 
    width: 50%; 
    background-color: #ffd700;
   }
</style>
<script>
import { fly } from 'svelte/transition';
import Detail from './Detail.svelte';
export let tooltipPos, hovering, selectedRegion, colFunc, percentage; 
let clientWidth; 
</script>

{#if hovering && selectedRegion}
<div bind:clientWidth={clientWidth} class="tooltip" in:fly={{duration:500}} style="top: {tooltipPos.y}px; left: {tooltipPos.x}px">
    <div class="tooltip-bar"></div>   
    <span class="tooltip-header">{selectedRegion.region}</span>
    <div class="tooltip-content">
    <div class="tooltip-row">
        <span class="row-lbl">Primary language:</span>
        <span>{selectedRegion.mainLang}</span>
    </div>
    
    <Detail {clientWidth} {colFunc} {selectedRegion}/>
</div>
</div>
{/if}

<style>
   .tooltip {
    border-radius: 5px;
    transition: .5s ease all; 
    pointer-events: none; 
    overflow:hidden;
    z-index: 2000;
    padding: 8px 6px;
    position: absolute; 
    min-width: 300px; 
    background-color: rgba(255,255,255,.9); 
    box-shadow: rgba(0, 0, 0, 0.15) 2px 3px 8px;
   }
   .tooltip-row {
    display: flex; 
    margin-bottom: .3rem; 
    justify-content: space-between;
   }

   .row-lbl {
    font-weight: 500; 
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
    background-color: rgb(115, 115, 247); 
   }
</style>
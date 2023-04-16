<script>
    export let guideLinePos=40, xScale; 
</script>

<line 
    y1={15} 
    y2={85} 
    stroke="firebrick" 
    stroke-width="2" 
    transform="translate({xScale(guideLinePos)},0)"
    x1={0} 
    x2={0}>

</line>

<style>
    line {
        transition: .5s cubic-bezier(0.33, 1, 0.68, 1) transform;
    }
</style>
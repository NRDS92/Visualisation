<script>
  import data from "./data/data.js";
  console.log(data);

  let width = 400;
  let height = 400;

  const margin = { top: 20, rigth: 40, left: 0, bottom: 20 };

  import { scaleLinear } from "d3-scale";
  $: xScale = scaleLinear()
    .domain([0, 100])
    .range([0, width - margin.left - margin.rigth]);

  import { max } from "d3-array";
  const yScale = scaleLinear()
    .domain([0, max(data, d => d.hours)])
    .range([height - margin.top - margin.bottom, 0]);

  import AxisX from "./components/AxisX.svelte";
  import AxisY from "./components/AxisY.svelte";
  import Tooltip from "./components/Tooltip.svelte";

  let hoveredData;
  $: console.log(hoveredData);
</script>
<h1>Study longer, score better</h1>

<div class="chart-container" 
      bind:clientWidth={width}
      on:mouseleave={ () => {
        hoveredData = null;
      }}>

  <svg {width} {height}>
    <AxisX {height} {margin} {xScale} />
    <AxisY {height} {width} {margin} {yScale}/>
      <g class="circles" transform="translate({margin.left}{margin.top} ) ">
        {#each data.sort((a,b) => a.grade - b.grade) as student}
          <circle cx={xScale(student.grade)} 
                  cy={yScale(student.hours)} 
                  r= {hoveredData && hoveredData == student? "20":"10" }
                  opacity= {hoveredData ? hoveredData == student? "1":"0.5": "1" }
                  fill="purple" 
                  stroke="blue"
                  on:mouseover={() => {
                    hoveredData = student;
                  }}
                  on:focus={ () =>{
                    hoveredData = student;  
                  }}
                  tabIndex="0"        
                  />
        {/each}
      </g>
  </svg>
  {#if hoveredData}
    <Tooltip data={hoveredData}  {xScale}{yScale} />
  {/if}
</div>

<style>
  circle {
    transition: all 300ms ease;
    cursor: pointer;
  }
  circle:focus {
    outline: none;
  }

  h1 {
    font-weight: 1.5rem;
    font-size: 600;
    margin-bottom: 0.5rem;
  }
</style>
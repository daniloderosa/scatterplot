<script>
  import data from "$data/data.js";
  import { each } from "svelte/internal";
  import { scaleLinear } from "d3-scale";
  import { max } from "d3-array";
  import AxisX from "$components/AxisX.svelte";
  import AxisY from "$components/AxisY.svelte";
  import Tooltip from "$components/Tooltip.svelte";
  let height = 400;
  let width = 400;
  const margin = {
    top: 20,
    right: 15,
    bottom: 20,
    left: 0,
  };
  $: innerWidth = width - margin.left - margin.right;
  const innerHeight = height - margin.top - margin.bottom;

  $: xScale = scaleLinear().domain([0, 100]).range([0, innerWidth]);

  let yScale = scaleLinear()
    .domain([0, max(data, (d) => d.hours)])
    .range([innerHeight, 0]);

  let hoveredData;
</script>

<h1>Students who studied longer scored higher on their final exams</h1>
<div class="chart-container" bind:clientWidth={width}>
  <svg {width} {height}>
    <g class="inner-chart" transform="translate({margin.left}, {margin.top})">
      <AxisY {yScale} width={innerWidth} />
      <AxisX height={innerHeight} {xScale} width={innerWidth} />
      {#each data.sort((a,b) => a.grade - b.grade) as d}
        <!-- svelte-ignore a11y-no-noninteractive-tabindex -->
        <circle
          cx={xScale(d.grade)}
          cy={yScale(d.hours)}
          r={hoveredData === d ? 20 : 10}
          opacity={hoveredData ? (hoveredData === d ? 1 : 0.5) : 1}
          fill="purple"
          stroke="black"
          stroke-width="1"
          on:mouseover={() => (hoveredData = d)}
          on:focus={() => (hoveredData = d)}
          on:mouseleave={() => hoveredData = null}
          tabindex="0"
        />
      {/each}
    </g></svg
  >
  {#if hoveredData}
    <Tooltip data={hoveredData} {xScale} {yScale} {width}></Tooltip>
  {/if}
</div>

<style>
  :global(.tick text, .axis-title) {
    font-weight: 400; /* How thick our text is */
    font-size: 12px; /* How big our text is */
    fill: hsla(212, 10%, 53%, 1); /* The color of our text */
  }

  circle {
    transition: r 200ms ease, opacity 500ms ease;
    cursor: pointer;
  }

  h1 {
    margin: 0 0 0.5rem 0;
    font-size: 1.35rem;
    font-weight: 600;
}
</style>

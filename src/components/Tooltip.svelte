<script>
    import { fly } from "svelte/transition";
    export let data;
    export let xScale;
    export let yScale;
    export let width;

    let tooltipWidth;

    $: x = xScale(data.grade);
    $: y = yScale(data.hours);
    const xNudge = 15;
    const yNudge = 25;

    $: xPosition =
        x + tooltipWidth > width ? x - tooltipWidth - xNudge : x + xNudge;
    $: yPosition = y + yNudge;
</script>

<div
    class="tooltip"
    transition:fly={{duration: 500}}
    style="position:absolute; top: {yPosition}px; left: {xPosition}px"
    bind:clientWidth={tooltipWidth}
>
    <h1>{data.name} <span>{data.grade}%</span></h1>
    <h2>{data.hours} hours studied</h2>
</div>

<style>
    .tooltip {
        padding: 8px 6px;
        background: white;
        box-shadow: rgba(0, 0, 0, 0.15) 2px 3px 8px;
        border-radius: 3px;
        pointer-events: none;
        transition:
            top 300ms ease,
            left 300ms ease;
    }

    h1 {
        font-size: 1rem;
        font-weight: 500;
        margin-bottom: 6px;
        width: 100%;
    }

    h2 {
        font-size: 0.8rem;
        font-weight: 300;
        text-transform: uppercase;
    }

    span {
        background: #dda0dd50;
        font-size: 80%;
        margin-left: 2px;
        padding: 2px 4px;
        display: inline-block;
        vertical-align: bottom;
        border-radius: 3px;
        color: rgba(0, 0, 0, 0.7);
    }
</style>

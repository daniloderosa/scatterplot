<script>
    import { each } from "svelte/internal";
    export let height;
    export let xScale;
    export let width;

    let xTicks = [0, 25, 50, 75, 100];
</script>

<g class="axis x" transform="translate(0,{height})">
    {#each xTicks as tick, index}
        <g class="tick" transform="translate({xScale(tick)},0)">
            <line x1="0" y1="0" x2="0" y2="6" stroke="hsla(212, 10%, 53%, 1)" />
            <text
                x="0"
                y="6"
                dy="9"
                dominant-baseline="middle"
                text-anchor={index === 0 ? "start" : "middle"}>{tick}%</text
            >
        </g>
    {/each}
    <text class="axis-title" x={width} text-anchor="end" y="-6">
        Final grade &rarr</text
    >
</g>

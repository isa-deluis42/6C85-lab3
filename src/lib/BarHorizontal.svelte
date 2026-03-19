<script>
    import * as d3 from 'd3';

    export let data = [];

    let width = 700;
    let height = 420;

    let xAxis;
    let yAxis;

    const margin = { top: 50, right: 140, bottom: 50, left: 100 };
    $: innerWidth = width - margin.left - margin.right;
    $: innerHeight = height - margin.top - margin.bottom;

    $: sortedData = [...data].sort((a, b) => b.value - a.value);

    $: maxDatum = sortedData.length ? sortedData[0] : null;

    $: xScale = d3.scaleLinear()
        .domain([0, d3.max(sortedData, d => d.value) || 1])
        .nice()
        .range([0, innerWidth]);

    $: yScale = d3.scaleBand()
        .domain(sortedData.map(d => d.label))
        .range([0, innerHeight])
        .padding(0.2);

    $: colorScale = d3.scaleOrdinal(d3.schemeTableau10)
        .domain(sortedData.map(d => d.label));

    $: if (xAxis) {
        d3.select(xAxis).call(d3.axisBottom(xScale).ticks(6).tickFormat(d3.format('d')));
    }

    $: if (yAxis) {
        d3.select(yAxis).call(d3.axisLeft(yScale));
    }

    $: maxBar = d3.greatest(data, d => d.value);
</script>

<div class="chart-wrap">
    <svg viewBox="0 0 {width} {height}">
        <!-- Title -->
        <text
            x={margin.left + innerWidth / 2}
            y={28}
            text-anchor="middle"
            class="chart-title"
        >
            Total Lines of Code per Language
        </text>

        <!-- X axis -->
        <g
            transform="translate({margin.left}, {margin.top + innerHeight})"
            bind:this={xAxis}
        />

        <!-- Y axis -->
        <g
            transform="translate({margin.left}, {margin.top})"
            bind:this={yAxis}
        />

        <!-- X axis label -->
        <text
            x={margin.left + innerWidth / 2}
            y={height - 10}
            text-anchor="middle"
            class="axis-label"
        >
            Total Lines of Code
        </text>

        <!-- Y axis label -->
        <text
            transform="translate(18, {margin.top + innerHeight / 2}) rotate(-90)"
            text-anchor="middle"
            class="axis-label"
        >
            Language
        </text>

        <!-- Bars -->
        <g transform="translate({margin.left}, {margin.top})">
            {#each sortedData as d}
                <rect
                    x={0}
                    y={yScale(d.label)}
                    width={xScale(d.value)}
                    height={yScale.bandwidth()}
                    fill={colorScale(d.label)}
                    rx="4"
                />
            {/each}

            <!-- Value labels at end of bars -->
            {#each sortedData as d}
                <text
                    x={xScale(d.value) + 6}
                    y={(yScale(d.label) ?? 0) + yScale.bandwidth() / 2}
                    dominant-baseline="middle"
                    class="value-label"
                >
                    {d.value}
                </text>
            {/each}

            <!-- Annotation for max bar -->
            {#if maxBar}
                <!-- outline around the max bar -->
                <rect
                    x={0}
                    y={yScale(maxBar.label)}
                    width={xScale(maxBar.value)}
                    height={yScale.bandwidth()}
                    fill="none"
                    stroke="black"
                    stroke-width="2"
                />

                <!-- leader line: vertical down from middle of bar -->
                <line
                    x1={xScale(maxBar.value) / 2}
                    y1={(yScale(maxBar.label) ?? 0) + yScale.bandwidth()}
                    x2={xScale(maxBar.value) / 2}
                    y2={(yScale(maxBar.label) ?? 0) + yScale.bandwidth() + 85}
                    stroke="black"
                    stroke-width="1.5"
                />

                <!-- annotation text -->
                <text
                    x={xScale(maxBar.value) / 2}
                    y={(yScale(maxBar.label) ?? 0) + yScale.bandwidth() + 100}
                    text-anchor="middle"
                    class="annotation"
                >
                    Most lines of code
                </text>
            {/if}
        </g>
    </svg>

    <ul class="legend">
        {#each sortedData as d}
            <li style="--color: {colorScale(d.label)}">
                <span class="swatch"></span>
                {d.label} <em>({d.value})</em>
            </li>
        {/each}
    </ul>
</div>

<style>
    .chart-wrap {
        display: flex;
        align-items: flex-start;
        gap: 1.5rem;
        margin: 2rem 0;
        flex-wrap: wrap;
    }

    svg {
        max-width: 100%;
        height: auto;
        overflow: visible;
        flex: 2;
        min-width: 320px;
    }

    .chart-title {
        font-size: 1.1rem;
        font-weight: 700;
        fill: currentColor;
    }

    .axis-label {
        font-size: 0.9rem;
        fill: currentColor;
    }

    .value-label {
        font-size: 0.8rem;
        fill: currentColor;
    }

    .annotation {
        font-size: 0.85rem;
        font-weight: 600;
        fill: currentColor;
    }

    .legend {
        list-style: none;
        padding: 0;
        margin: 0;
        min-width: 180px;
        flex: 1;
    }

    .legend li {
        display: flex;
        align-items: center;
        margin-bottom: 0.45rem;
    }

    .swatch {
        display: inline-block;
        width: 12px;
        height: 12px;
        margin-right: 0.5rem;
        background-color: var(--color);
        border: 1px solid #999;
        flex-shrink: 0;
    }

    .annotation {
        font-size: 0.7em;
        fill: black;
        font-style: italic;
    }
</style>
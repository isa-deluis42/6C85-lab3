<script>
    import { onMount } from 'svelte';
    import * as d3 from 'd3';
    import BarHorizontal from '$lib/BarHorizontal.svelte';

    import { base } from '$app/paths';
    
    let locData = [];
    let chartData = [];

    onMount(async () => {
        locData = await d3.csv(`${base}/loc.csv`, row => ({
            ...row,
            line: Number(row.line),
            length: Number(row.length),
            depth: Number(row.depth)
        }));

        chartData = d3.rollups(
            locData,
            v => v.length,
            d => d.type
        )
        .map(([label, value]) => ({ label, value }))
        .sort((a, b) => b.value - a.value);
    });
</script>

<h1>Meta</h1>
<p>This page shows a breakdown of my codebase by language.</p>

<BarHorizontal data={chartData} />

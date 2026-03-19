<script>
    import projects from "$lib/projects.json";
    import Project from "$lib/Project.svelte";
    import ProjectNarrative from "$lib/ProjectNarrative.svelte";
    import Bar from '$lib/Bar.svelte';
    import { onMount } from 'svelte';
    import * as d3 from 'd3';

    let years = projects.map(proj => proj.year)

    let range = Math.max(...years) - Math.min(...years);

    let rawData = [];

    let wrangled = [];

    onMount(async () => {
        rawData = await d3.json('/lab6_example.json');
        wrangled = d3.rollups(
            rawData,
            v => d3.sum(v, d => d.lines),
            d => d.language
        );
    });

    $: barData = d3.rollups(projects, v => v.length, d => d.year)
        .map(([year, count]) => ({ label: String(year), value: count }));

</script>


    <Bar data={barData}/>

    <h1> {projects.length} Projects in {range} Years: My Research Story</h1>
        <p>Scroll down to see a timeline of my projects!</p>
        <ProjectNarrative />
        <p class = "outro"> Thanks for reading!</p>
        <div class="projects">
            {#each projects as p}
                <Project data={p} />
            {/each}
        </div>

<style>
    .projects {
        display: grid;
        grid-template-columns: repeat(auto-fill, minmax(15em, 1fr));
        gap: 1em;
  }

    .outro {
        margin-bottom: 10rem;
    }

    


  
</style>
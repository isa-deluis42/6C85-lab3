<script>
    import projects from "$lib/projects.json";
    import Project from "$lib/Project.svelte";
    import ProjectNarrative from "$lib/ProjectNarrative.svelte";
    

    let years = projects.map(proj => proj.year)

    let range = Math.max(...years) - Math.min(...years);

</script>
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
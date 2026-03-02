<script>
  import projects from "$lib/projects.json";
  import Project from "$lib/Project.svelte";
</script>
    <h1> Projects</h1>
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

  
</style>
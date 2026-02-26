<script>
  import projects from "$lib/projects.json";
  import Project from "$lib/Project.svelte";
</script>
  <nav class = "menu">
    <ul>
        <li> <a href = "../">Home</a></li>
        <li> <a href = "../projects" class = "current">Projects</a></li>
        <li> <a href = "../contact">Contact</a></li>
        <li> <a href = "../resume"> CV </a></li>
        <li> <a href = "https://github.com/isa-deluis42" target="_blank">Github</a></li>
    </ul>
</nav>
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
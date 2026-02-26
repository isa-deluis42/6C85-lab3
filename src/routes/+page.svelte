<script>
  import projects from "$lib/projects.json";
  import Project from "$lib/Project.svelte";
  import readings from "$lib/reading.json";
  import ReadingItem from "$lib/ReadingItem.svelte";

</script>
<nav class = "menu">
    <ul>
        <li> <a href = "." class = "current">Home</a></li>
        <li> <a href = "projects">Projects</a></li>
        <li> <a href = "contact">Contact</a></li>
        <li> <a href = "resume"> CV </a></li>
        <li> <a href = "https://github.com/isa-deluis42" target="_blank">Github</a></li>
    </ul>
</nav>
<h1> Isabel de Luis </h1>
    <p> I’m a first-year graduate student at MIT AeroAstro, researching with 
        Prof. Julie Shah’s Interactive Robotics Group and Dr. Ben Armstrong 
        at the Industrial Performance Center. I received my Bachelor’s of 
        Science in Engineering from Olin College of Engineering in May 2025.
        My research is in human-computer interaction, driven by a commitment 
        to improving the safety and transparency of intelligent systems. 
        I’ve previously worked on projects at Caltech, MIT, and JPL exploring 
        human factors and autonomy in aviation and space exploration.</p>
    <img
    src="images/image_me.jpeg"
    alt = "Isa de Luis with statue of Charles Darwin"/>
<h1>Latest Projects</h1>

<div class="projects">
    {#each projects.slice(0, 3) as p}
        <Project data={p} />
    {/each}
</div>

<h1> Currently Reading</h1>
    
<div class="reading">
    {#each readings as r}
        <ReadingItem data={r} />
    {/each}
</div>

<style>
    .projects {
        display: grid;
        grid-template-columns: repeat(auto-fill, minmax(15em, 1fr));
        gap: 1em;
  }

   .reading {
        display: grid;
        grid-template-columns: repeat(auto-fill, minmax(15em, 1fr));
        gap: 1em;
  }
</style>
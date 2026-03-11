<script>
    import projects from "$lib/projects.json";
    import Project from "$lib/Project.svelte";
    import readings from "$lib/reading.json";
    import ReadingItem from "$lib/ReadingItem.svelte";
    import { onMount } from "svelte";

    let githubData = null; // This will eventually hold our Github stats
    let loading = true; // This will be true *until* the fetch's promise resolves to a value
    let error = null; // If the API call resulted in an error, it will go into this variable

    onMount(async () => {
        try {
            console.log("Page has been mounted!")
            let response = await fetch("https://api.github.com/users/isa-deluis42");
            console.log(response);
            githubData = await response.json();
            console.log(githubData);
        } catch (err) {
            error = err;
        }
        
        loading = false;
    }
)


</script>

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
    {#if loading}
        <p>Loading...</p>
    {:else if error}
        <p>Something went wrong: {error.message}</p>
    {:else}
        <section class = "github">
            <h2>Github Stats</h2>
            <dl class = "github-stats">
                <div class = "stat">
                    <dt>Followers</dt>
                    <dd>{githubData.followers}</dd>
                </div>
                
                <div class="stat">
                    <dt>Following</dt>
                    <dd>{githubData.following}</dd>
                </div>

                <div class="stat">
                    <dt>Public Repos</dt>
                    <dd>{githubData.public_repos}</dd>
                </div>
            </dl>
        </section>
    {/if}
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
  .github {
        padding: 2rem;
        margin: 2rem 0;
    }

    .github h2 {
        font-size: 2rem;
        margin-bottom: 1.5rem;
    }

    .github-stats {
        display: grid;
        grid-template-columns: repeat(auto-fit, minmax(12rem, 1fr));
        gap: 2rem;
    }

    .stat {
        text-align: left;
    }

    .stat dt{
        font-size: 0.85rem;
        letter-spacing: 0.08em;
        text-transform: uppercase;
        color: #6b7280;
        margin-bottom: 0.25rem;
    }

   .stat dd {
        font-size: 3rem;
        font-weight: 300;
        margin: 0;
    }
</style>
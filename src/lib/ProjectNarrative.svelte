<script>
    import Scrolly from "svelte-scrolly";
	import projects from "$lib/projects.json";
	  
    let scrollyProgress = 0
	let sorted_projects = projects.sort((a, b) => a.year - b.year)
	let progressPerProject = 100 / sorted_projects.length;

	$: activeProjectIdx = 
		Math.min(sorted_projects.length-1, Math.floor(scrollyProgress / progressPerProject));


</script>

<Scrolly bind:progress={ scrollyProgress }>
	<!-- Story here -->
	 <div class="scrolly-wrapper">
			
				{#each sorted_projects as p}
				<section class="step">
					<h3>{p.title}</h3>
					<div class="step-content">
					<p>{p.story}</p>
					</div>
				</section>
					
				{/each}	

						
	 	
	 </div>
	 
	<svelte:fragment slot="viz">
		<!-- Visualizations here (these will stay sticky) -->
		<div class="project-detail">
			<h3>{sorted_projects[activeProjectIdx].year}</h3>
			<img src = {sorted_projects[activeProjectIdx].image} 
				alt = ""/>
		</div>
		 
	</svelte:fragment>

</Scrolly>

<style>
	.scrolly-wrapper {
		/* width: min(800ch, 30vw); */
		position: relative;
		left: 50%;
		transform: translateX(-50%);
}
	.step {
		/* color: oklch(65% 50% 0); */
		min-height: 80vh;
		padding: 2rem;
	}

	.step-content{
		padding: 1.5em 2rem;
		background-color: color-mix(in oklch, var(--color-accent), canvas 85%);
	}
	h3 {
		color: oklch(65% 50% 0);
	}
	
</style>
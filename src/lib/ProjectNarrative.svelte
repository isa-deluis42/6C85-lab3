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
		<section class="step">
			
				{#each sorted_projects as p}
				<h3>{p.title}</h3>
				<div class="step-content">
					
					<p>{p.story}</p>
				</div>
					
				{/each}
	 	</section>
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
		width: min(1000ch, 60vw);
		position: relative;
		left: 50%;
		transform: translateX(-50%);
}
	.step {
		min-height: 80vh;
		padding: 2rem;
	}

	.step-content{
		padding: 1.5rem 2rem;
	}
	.project-detail{
		padding: 2rem;
		width: 100%;
	}
	img {
		width: 100%;
	}
</style>
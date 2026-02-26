<script>
    export let data = {};
</script>
<article>
    <h2>{data.title}</h2>
    <img src={data.image} alt="">
    <p>{data.description}</p>
</article>

<style>
  .reading {
        display: grid;
        grid-template-columns: repeat(auto-fill, minmax(15em, 1fr));
        gap: 1em;
  }

  article {
        display: grid;
        grid-template-rows: subgrid;
        grid-row: span 3;
    }
    h2 {
        margin: 0;
    }
    
</style>
<script>
    export let data = {};
</script>
<article>
    <h2>{data.title}</h2>
    
    <img src={data.image} alt="">
    <i>{data.year}</i>
    <p>{data.description}</p>
</article>

<style>
    article {
        display: grid;
        grid-template-rows: subgrid;
        grid-row: span 4;
    }
    h2 {
        margin: 0;
    }
</style>
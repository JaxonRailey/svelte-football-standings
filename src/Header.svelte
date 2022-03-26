<script>

    export let league;
    export let loader;
           let src;

    $: {
        if (!league) league = '';
        let slug = league.trim().toLowerCase().replace(/ /g, '-').replace(/[^\w-]+/g, '');
        src = 'img/league/' + slug + '.png';
    }

</script>

<header {loader}>
    <img {src} alt="{league}">
    <h1>{league}</h1>
</header>

<style>

    img {
        height: 40px;
        position: absolute;
        top: 2px;
    }

</style>
<script>

    export let league;
    export let nation;
           let src = {};

    $: {
        let slug   = league.trim().toLowerCase().replace(/ /g, '-').replace(/[^\w-]+/g, '');
        src.league = 'img/league/' + slug + '.png';
        src.nation = 'img/nation/' + nation + '.png';
    }

</script>

<header>
    <img src={src.nation} alt={nation}>
    <h1>{league}</h1>
    <img src={src.league} alt={league}>
</header>

<style>

    img {
        height: 40px;
        margin-top: 2px;
    }

</style>
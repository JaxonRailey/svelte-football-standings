<script>

    import Header from './Header.svelte';
    import Loader from './Loader.svelte';
    import Footer from './Footer.svelte';

    let league = 'ita';
    let title  = null;
    let loader = true;
    let reqs   = getData();

    async function getData() {
        const response = await fetch('https://api-football-standings.azharimm.site/leagues/' + league + '.1/standings?season=2021');
        const data     = await response.json();
              title    = data.data.name;
              title    = title.substring(title.indexOf(' ') + 1);
              loader   = false;

        return data;
    }

    function changeLeague(page) {
        title  = '';
        loader = true;
        league = page.detail;
        reqs   = getData();
    }

</script>

<Header league={title} loader={loader} />

<main {loader}>
    {#await reqs}
        <Loader />
    {:then data}
        <ul class="table-view">
            {#each data.data.standings as team}
            <li class="table-view-cell">{team.team.name}
                <span>{team.stats[6].value}</span>
            </li>
            {/each}
        </ul>
    {:catch error}
        <p>An error occurred!</p>
    {/await}
</main>

<Footer league={league} loader={loader} on:changeLeague={changeLeague} />

<style>

    :global([loader="true"]) {
        visibility: hidden;
    }

    main[loader="true"] {
        visibility: visible;
        display: flex;
        width: 100%;
        height: 100%;
        align-items: center;
        justify-content: center;
        padding: 0;
    }

</style>
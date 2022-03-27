<script>

    import { fade } from 'svelte/transition';

    import Header from './Header.svelte';
    import Footer from './Footer.svelte';

    let nations = ['ita', 'eng', 'esp', 'fra', 'ger'];
    let leagues = ['Serie A', 'Premier League', 'LaLiga', 'Ligue 1', 'Bundesliga'];
    let nation  = nations[0];
    let league  = leagues[0];
    let reqs    = getData();

    async function getData() {
        const response = await fetch('https://api-football-standings.azharimm.site/leagues/' + nation + '.1/standings?season=2021');
        const data     = await response.json();

        if (data.status === true) {
            return data.data;
        }

        throw new Error(data.data);
    }

    function changeLeague(page) {
        nation = page.detail;
        league = leagues[nations.indexOf(nation)];
        reqs   = getData();
    }

</script>

<Header league={league} nation={nation} />

<main>
    {#await reqs then data}
        <ul class="table-view" transition:fade>
            {#each data.standings as team}
            <li class="table-view-cell">{team.team.name}
                <span>{team.stats[6].value}</span>
            </li>
            {/each}
        </ul>
    {:catch}
        <p transition:fade>An error occurred!</p>
    {/await}
</main>

<Footer nation={nation} nations={nations} on:changeLeague={changeLeague} />

<style>

    p {
        display: flex;
        height: 100%;
        width: 100%;
        align-items: center;
        justify-content: center;
        font-size: 18px;
        text-align: center;
    }

</style>
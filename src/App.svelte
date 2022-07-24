<script>

    import { fade } from 'svelte/transition';

    import Header from './Header.svelte';
    import Footer from './Footer.svelte';

    let nations = ['ita', 'eng', 'esp', 'fra', 'ger'];
    let leagues = ['Serie A', 'Premier League', 'LaLiga', 'Ligue 1', 'Bundesliga'];
    let nation  = nations[0];
    let league  = leagues[0];
    let year    = new Date().getFullYear() - 1;
    let maxYear = year;
    let reqs    = getData();

    if (new Date().getMonth() > 5) {
        year    = new Date().getFullYear();
        maxYear = year;
    }

    async function getData() {
        const response = await fetch('https://api-football-standings.azharimm.site/leagues/' + nation + '.1/standings?season=' + year);
        const data     = await response.json();

        if (data.status === true) {
            let rows = data.data;

            rows.standings.map((team, index) => {
                if (team.note) {
                    switch (team.note.description) {
                        case 'Champions League': rows.standings[index].note.color = '#34A853'; break;
                        case 'Champions League qualifying': rows.standings[index].note.color = '#8dd175'; break;
                        case 'Europa League': rows.standings[index].note.color = '#4285F4'; break;
                        case 'UEFA Cup': rows.standings[index].note.color = '#4285F4'; break;
                        case 'Europa League qualifying': rows.standings[index].note.color = '#17DEFA'; break;
                        case 'Europa Conference League': rows.standings[index].note.color = '#FA7B17'; break;
                        case 'Europa Conference League qualifying': rows.standings[index].note.color = '#FA7B17'; break;
                        case 'Relegated': rows.standings[index].note.color = '#fe6366'; break;
                        case 'Relegation': rows.standings[index].note.color = '#fe6366'; break;
                    }
                }
            });

            return rows;
        }

        throw new Error(data.data);
    }

    function changeLeague(page) {
        nation = page.detail;
        league = leagues[nations.indexOf(nation)];
        reqs   = getData();
    }

    function changeYear(data) {
        if (data.detail <= maxYear && data.detail > 2000) {
            year = data.detail;
            reqs = getData();
        }
    }

</script>

<Header league={league} nation={nation} on:changeYear={changeYear} year={year} />

<main>
    {#await reqs then data}
        <ul class="table-view" transition:fade>
            <li class="table-view-cell">
                <span>#</span>
                <span>Squadra</span>
                <span>PT</span>
                <span>PG</span>
                <span>V</span>
                <span>P</span>
                <span>S</span>
                <span>GF</span>
                <span>GS</span>
            </li>
            {#each data.standings as team, index}
            <li class="table-view-cell">
                <span>{index + 1}</span>
                <span style="color: {team.note ? team.note.color : ''}">{team.team.name}</span>
                <span>{team.stats[6].value}</span>
                <span>{team.stats[3].value}</span>
                <span>{team.stats[0].value}</span>
                <span>{team.stats[1].value}</span>
                <span>{team.stats[2].value}</span>
                <span>{team.stats[4].value}</span>
                <span>{team.stats[5].value}</span>
            </li>
            {/each}
        </ul>
    {:catch}
        <p transition:fade>An error occurred!</p>
    {/await}
</main>

<Footer nation={nation} nations={nations} on:changeLeague={changeLeague} />

<style>

    main {
        position: absolute;
        top: 0;
        right: 0;
        bottom: 0;
        left: 0;
        overflow: auto;
        -webkit-overflow-scrolling: touch;
        background-color: #fff;
        padding-top: 44px;
        height: calc(100% - 94px);
    }

    ul {
        padding: 0;
        margin: 0;
        margin-top: 3px;
    }

    li {
        padding: 11px 15px;
        position: relative;
        overflow: hidden;
        border-bottom: 1px solid #ddd;
        position: relative;
        top: 38px;
    }

    li:first-child {
        font-weight: bold;
        position: fixed;
        top: 45px;
        background-color: #fff;
        z-index: 2;
        width: 100%;
        text-transform: uppercase;
    }

    li:first-child span {
        font-size: 11px !important;
    }

    li span:nth-child(1) {
        width: 12px;
        margin-right: 10px;
    }

    li span:nth-child(2) {
        font-size: 12px;
        color: #333;
        display: inline-block;
        font-weight: bold;
        width: calc(100vw - 235px);
        text-align: left;
        white-space: nowrap;
        overflow: hidden;
        text-overflow: ellipsis;
        top: 3px;
        position: relative;
    }

    li span {
        font-size: 11px;
        color: #333;
        display: inline-block;
        width: 22px;
        text-align: right;
    }

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
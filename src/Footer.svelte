<script>

    import { createEventDispatcher } from 'svelte';

    export let nation   = null;
    export let nations  = [];
           let dispatch = createEventDispatcher();

    function changeLeague(nation) {
        dispatch('changeLeague', nation);
    }

</script>

<nav>
    {#each nations as n}
        <a href={n} class:active="{nation === n}" on:click|preventDefault={() => changeLeague(n)}>
            <img src="img/nation/{n}.png" alt={n}>
        </a>
    {/each}
</nav>

<style>

    nav {
        bottom: 0;
        display: table;
        width: 100%;
        height: 50px;
        padding: 0;
        table-layout: fixed;
        border-top: 1px solid #ddd;
        border-bottom: 0;
        position: fixed;
        background-color: white;
        cursor: pointer;
    }

    nav a {
        cursor: pointer;
        display: table-cell;
        text-align: center;
        vertical-align: middle;
    }

    nav img {
        height: 36px;
    }

</style>
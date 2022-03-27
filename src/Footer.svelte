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

    nav img {
        height: 36px;
    }

</style>
<script>

    import { createEventDispatcher } from 'svelte';

    export let league   = null;
    export let loader   = false;
           let dispatch = createEventDispatcher();

    function changeLeague(league) {
        dispatch('changeLeague', league);
    }

</script>

<nav {loader}>
    <a class:active="{league === 'ita'}" on:click={() => changeLeague('ita')}>
        <img src="img/flag/ita.png" alt="Italy">
    </a>
    <a class:active="{league === 'eng'}" on:click={() => changeLeague('eng')}>
        <img src="img/flag/eng.png" alt="England">
    </a>
    <a class:active="{league === 'esp'}" on:click={() => changeLeague('esp')}>
        <img src="img/flag/esp.png" alt="Espana">
    </a>
    <a class:active="{league === 'fra'}" on:click={() => changeLeague('fra')}>
        <img src="img/flag/fra.png" alt="France">
    </a>
    <a class:active="{league === 'ger'}" on:click={() => changeLeague('ger')}>
        <img src="img/flag/ger.png" alt="Germany">
    </a>
</nav>

<style>
    nav img {
        height: 36px;
    }
</style>
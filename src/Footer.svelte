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
        ITA
    </a>
    <a class:active="{league === 'eng'}" on:click={() => changeLeague('eng')}>
        ENG
    </a>
    <a class:active="{league === 'esp'}" on:click={() => changeLeague('esp')}>
        ESP
    </a>
    <a class:active="{league === 'fra'}" on:click={() => changeLeague('fra')}>
        FRA
    </a>
    <a class:active="{league === 'ger'}" on:click={() => changeLeague('ger')}>
        GER
    </a>
</nav>
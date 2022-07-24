<script>

    import { createEventDispatcher } from 'svelte';

    export let league;
    export let nation;
    export let year;
           let dispatch = createEventDispatcher();
           let src = {};

    $: {
        let slug   = league.trim().toLowerCase().replace(/ /g, '-').replace(/[^\w-]+/g, '');
        src.league = 'img/league/' + slug + '.png';
        src.nation = 'img/nation/' + nation + '.png';
    }

    function changeYear(year) {
        dispatch('changeYear', year);
    }

</script>

<header>
    <div class="flex">
        <div>
            <img src={src.league} alt={league}>
        </div>

        <div>
            <h1>{league}</h1>
        </div>

        <div>
            <ul>
                <li on:click|preventDefault={() => changeYear(year - 1)}> &#xab; </li>
                <li>{year}</li>
                <li on:click|preventDefault={() => changeYear(year + 1)}> &#xbb; </li>
            </ul>
        </div>
    </div>
</header>

<style>

    header {
        position: fixed;
        right: 0;
        left: 0;
        z-index: 10;
        height: 44px;
        padding-right: 10px;
        padding-left: 10px;
        background-color: white;
        border-bottom: 1px solid #ddd;
        display: flex;
        justify-content: space-between;
    }

    h1 {
        margin: 0 -10px;
        font-size: 17px;
        font-weight: 500;
        line-height: 44px;
        color: #000;
        text-align: center;
        white-space: nowrap;
    }

    img {
        height: 40px;
        margin-top: 2px;
    }

    ul {
        display: flex;
        list-style: none;
        justify-content: end;
    }

    ul li:first-child,
    ul li:last-child {
        padding: 0 10px;
        cursor: pointer;
        font-weight: bold;
    }

    .flex {
        display: flex;
        width: 100%;
    }

    .flex > div {
        width: 33%;
    }

</style>
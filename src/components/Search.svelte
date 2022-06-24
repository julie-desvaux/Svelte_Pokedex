<script>
    import {createEventDispatcher} from "svelte"

    const dispatch = createEventDispatcher()

    let search = ""

    const handleSearch = () => {
        dispatch('search-pokemon', {poke: search})
        search = ""
    }
</script>

<form class="search-form" on:submit|preventDefault={handleSearch}>
    <label for="search">Rechercher un pokémon</label>
    <input bind:value={search} id="search" type="text" placeholder="Pikachu, Salamèche, ...">
    <button type="submit">Rechercher</button>
</form>

<style>
    .search-form {
        margin: 100px auto 50px auto;
        max-width: 1100px;
        width: 100%;
        padding: 0 50px;
        display: flex;
        flex-direction: column;
        align-items: center;
    }
    .search-form input, .search-form label {
        max-width: 1100px;
        width: 100%;
    }
    .search-form label {
        font-size: 1.5rem;
        border: none;
        padding: 10px 0px;
    }
    .search-form input {
        font-size: 20px;
        padding: 15px 20px;
        background: rgb(240,197,240);
        border: 2px solid violet;
        border-radius: 5px;
    }
    .search-form button {
        padding: 15px 30px;
        background: violet;
        border: 2px solid rgb(240,197,240);
        border-radius: 10px;
        outline: none;
        cursor: pointer;
        margin-top: 5px;
        font-size: 1.2rem;
        margin-right: auto;
    }
</style>
<script>
    import {v4 as uuidv4} from "uuid";
	import Search from "./Search.svelte";
    import Card from "./Card.svelte";

    let allPokemons = [];
    let arrayEnd = [];
    let beggin = 0;
    let contentSearch = ""

    const fetchPokemons = () => {
        fetch("https://pokeapi.co/api/v2/pokemon?limit=151")
            .then((res) => res.json())
            .then((res) => {
                res.results.forEach(pokemon => {
                    fetchPokemonData(pokemon)
                });
            })
    }
    
    const fetchPokemonData = (pokemon) => {
        let objPokemon = {};
        let url = pokemon.url;
        let namePokemon = pokemon.name
        fetch(url)
            .then((res) => res.json())
            .then((res) => objPokemon.pic = res.sprites.front_default)
        fetch(`https://pokeapi.co/api/v2/pokemon-species/${namePokemon}`)
            .then((res) => res.json())
            .then((res) => {
                objPokemon.name = res.names[4].name
                objPokemon.id = res.id
                allPokemons.push(objPokemon)
            })
            .then(() => arrayEnd = allPokemons.slice(beggin,20))
    }    
    
    fetchPokemons()

    const nextPokemons = () => {
        if( beggin + 20 <= 151)
        beggin += 20;
        arrayEnd = allPokemons.slice(beggin,beggin + 20)
        window.scrollTo({top: 0, behavior: 'smooth'});
    }

    const prevPokemons = () => {
        if( beggin - 20 >= 0)
        beggin -= 20;
        arrayEnd = allPokemons.slice(beggin,beggin + 20)
        window.scrollTo({top: 0, behavior: 'smooth'});

    }

    const resetSearch = () => {
        contentSearch = ""
        beggin = 0
        arrayEnd = allPokemons.slice(beggin,20)
    }

    const searchFunc = (event) => {
        contentSearch = event.detail.poke
        arrayEnd = allPokemons.filter((pokemon) => pokemon.name.toLowerCase().includes(contentSearch.toLowerCase())
        )
        console.log(arrayEnd.length);
    }

</script>

<Search on:search-pokemon={searchFunc}/> 
<div class="content">
    {#each arrayEnd as pokemon (uuidv4())}
        <Card name={pokemon.name} pic={pokemon.pic} id={pokemon.id}/>
    {/each}
    {#if (contentSearch.length != 0 && arrayEnd.length == 0)}
        <p>Merci d'écrire le nom d'un pokémon de la première génération</p>
    {/if}

    
</div>

<div class="container-btns">
    {#if (contentSearch.length != 0)}
        <button on:click={resetSearch}>Reset</button>
    {:else}
        {#if (beggin > 0)}
            <button on:click={prevPokemons}>Précédent</button>
        {/if}
        {#if (beggin < 151)}
            <button on:click={nextPokemons}>Suivant</button>
        {/if}
    {/if}
</div>

<style>
    .content {
        max-width: 1200px;
        width: 95%;
        padding: 0 50px;
        margin: 0 auto;
        display: flex;
        flex-wrap: wrap;
        justify-content: center;
    }
    .container-btns {
        max-width: 1200px;
        width: 95%;
        padding: 0 50px;
        margin: 20px auto;
        display: flex;
        justify-content: center;
        gap: 25px
    }
    .container-btns button {
        padding: 15px 30px;
        background: violet;
        border: 2px solid rgb(240,197,240);
        border-radius: 10px;
        outline: none;
        cursor: pointer;
        margin-top: 5px;
        font-size: 1.2rem;
    }
</style>
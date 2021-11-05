<script context="module">
    export async function load({page}){
        const url = `https://pokeapi.co/api/v2/pokemon?limit=150`;
    const res = await fetch(url);
    const data = await res.json();
    const loadedPokemon = data.results.map((data, index) => {
        return {
            name: data.name,
            id: index + 1,
            image: `https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${index + 1
                }.png`
        };
    });
    return {props:{pokemon:loadedPokemon}}
    }
   </script>
<script>
    
     import PokemanCard from "/home/nazreenshamji/Desktop/svelte2/demo/mysveltekit-pokedex2/src/components/pokemanCard.svelte";
     export let pokemon;
    // console.log($pokemon);
    let searchTerm="";
    let filteredPokemon = [];

    $:{
        console.log(searchTerm);
        if (searchTerm) {
            //search the pokemon
            filteredPokemon = pokemon.filter(pokeman => pokeman.name.toLowerCase().includes(searchTerm.toLowerCase()));          
        } else {
            filteredPokemon= [...pokemon]
        }
    }
    </script>
    <svelte:head>
        <title>SvelteKit Pokedex</title>
    </svelte:head>
    
    <h1 class="text-center text-4xl my-8 uppercase">SvelteKit Pokedex</h1>

    <input class="w-full rounded-md text-lg p-4 border-2 border-green-200" type="text" bind:value="{searchTerm}" placeholder="Search Pokemon">
    
    <div class="py-4 grid gap-4 md:grid-cols-2 grid-cols-1">
        {#each filteredPokemon as pokeman}
        <PokemanCard pokeman={pokeman}/>
        {/each}
    </div>
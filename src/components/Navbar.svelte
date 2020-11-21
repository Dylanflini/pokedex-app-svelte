<script>
  import {
isResultVisibleStore,
    pokemonSearchesStore,
    pokemonsFilterStore,
    typesStore,
    typeStore,
  } from "./PokemonStore.svelte";
  import Search, { ALL_TYPE } from "./Search.svelte";

  let isResultVisible = true,
    pokemonsFound,
    type = ALL_TYPE,
    options,
    pokemonSearches = "";

  isResultVisibleStore.subscribe((value) => {
    isResultVisible = value;
  });

  pokemonsFilterStore.subscribe((value) => {
    pokemonsFound = value.length;
  });

  typesStore.subscribe((value) => {
    options = value;
  });

  $: {
    typeStore.set(type);
  }

  $: {
    pokemonSearchesStore.set(pokemonSearches);
  }
</script>

<style>
  nav {
    position: fixed;
    z-index: 500;
    width: 100%;
    padding: 10px;
    background-color: rgb(224, 0, 48);
    color: white;
    box-shadow: 0 3px 1px -1px rgba(0, 0, 0, 0.2),
      0 1px 1px 0 rgba(0, 0, 0, 0.14), 0 1px 3px 0 rgba(231, 148, 148, 0.12);
  }
  ul {
    list-style: none;
    margin: 0;
    padding: 0;
    width: 100%;
    max-width: 1200px;
    margin: 0 auto;

    display: flex;
    flex-wrap: wrap;
    align-items: center;
  }

  li {
    margin: 0;
    padding: 0;

    list-style: none;
    flex-grow: 0;
    flex-shrink: 0;
    display: flex;
    align-items: center;
  }

  img {
    width: 50px;
  }

  img:hover {
    cursor: pointer;
  }
</style>

<nav>
  <ul>
    <li>
      <a href="/">
        <img src="/pokemon_icon.svg" alt="Logo de Pokedex App" />
      </a>
    </li>
    <li>
      <Search bind:pokemonSearches {options} bind:type />
    </li>

    {#if isResultVisible}
      <li>
        <p>Pokemons found: {pokemonsFound}</p>
      </li>
    {/if}
  </ul>
</nav>

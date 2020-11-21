<script>
  import Navbar from "$components/Navbar.svelte";
  import Spinner from "$components/Spinner.svelte";
  import UsePokemons from "$components/usePokemons.svelte";
  import { isLoadingStore, typeStore } from "$components/PokemonStore";
  import {
    pokemonSearchesStore,
    pokemonsFilterStore,
  } from "../components/PokemonStore.svelte";

  // let types = [],;
  let pokemonSearches = "",
    types = [],
    type = "",
    // isResultVisible = true,
    // limit = 20,
    pokemonsFilter = [],
    isLoading = false;

  pokemonsFilterStore.subscribe((value) => {
    pokemonsFilter = value;
  });

  pokemonSearchesStore.subscribe((value) => {
    pokemonSearches = value;
  });

  isLoadingStore.subscribe((value) => {
    isLoading = value;
  });

  typeStore.subscribe((value) => {
    type = value;
  });

  // $: console.log("pokemonsFilter", pokemonsFilter);

  // $: console.log("pokemons ", pokemons);
  // $: console.log("pokemon searches ", pokemonSearches);
  // // $: console.log("pokemon filter ", pokemonsFilter);
  // $: console.log("types ", types);
  // $: console.log("type ", type);
  // $: console.log("isloading ", isLoading);
</script>

<style>
  main {
    padding-top: 60px;
  }
</style>

<UsePokemons {type} {pokemonSearches} />

<Navbar />

<main>
  <slot />
  {#if isLoading}
    <Spinner width="50px" />
  {/if}
</main>

<script>
  import {
    pokemonSearchesStore,
    pokemonsFilterStore,
    isLoadingStore,
    pokemonsStore,
    typesStore,
    typeStore,
  } from "./PokemonStore.svelte";
  import { onMount } from "svelte";
  import {
applyAllFilter,
    fetchByTypes,
    fetchPokemonTypes,
    mergeTypes,
    sortLessToGreater,
  } from "./Utils.svelte";

  export let type,
    pokemonSearches,
    limit = 20,
    offset = 0;
  // sort = [];

  export let pokemonsFilter = [],
    count = 0;

  let pokemons = [],
    types = [];

  pokemonsStore.subscribe((value) => {
    pokemons = value;
  });

  typesStore.subscribe((value) => {
    types = value;
  });

  typeStore.subscribe((value) => {
    type = value;
  });

  pokemonSearchesStore.subscribe((value) => {
    pokemonSearches = value;
  });

  onMount(() => {
    (async () => {
      typesStore.set(await fetchPokemonTypes());
    })();
  });


  $: {
    (async () => {
      if (types.length > 0 && count === 0) {
        count = 1;

        isLoadingStore.set(true);

        pokemonsStore.set(
          mergeTypes(await fetchByTypes(types, [])).sort(sortLessToGreater)
        );

        isLoadingStore.set(false);
      }
    })();
  }

  $: {
    if (types.length > 0 && pokemons.length > 0) {
      pokemonsFilterStore.set(
        applyAllFilter(
          pokemonsFilter,
          pokemons,
          pokemonSearches,
          type,
          offset,
          limit
        )
      );
    }
  }
</script>

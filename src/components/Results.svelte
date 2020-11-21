<script>
  import Pokemon from "./Pokemon.svelte";
  import {
    isResultVisibleStore,
    pokemonsFilterStore,
  } from "./PokemonStore.svelte";

  let pokemons = [],
    isResultVisible = true;

  pokemonsFilterStore.subscribe((value) => {
    pokemons = value;
  });

  isResultVisibleStore.subscribe((value) => {
    isResultVisible = value;
  });
</script>

<style>
  div {
    --auto-grid-min-size: 9rem;
    background: white;
    min-height: 85vh;
    display: grid;
    grid-template-columns: repeat(
      auto-fill,
      minmax(var(--auto-grid-min-size), 1fr)
    );
    grid-template-rows: repeat(4, min-content);
    grid-gap: 1rem;
    padding: 1rem;
    padding-top: 2.5rem;
    padding-bottom: 3.5rem;
  }
</style>

{#if isResultVisible}
  <div>
    {#each pokemons as { name, id, types }}
      <Pokemon {name} {id} {types} />
    {/each}
  </div>
{/if}

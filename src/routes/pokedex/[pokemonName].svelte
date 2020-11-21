<script context="module">
  export async function preload(page) {
    const { params, query } = page;

    return { params, query };
  }
</script>

<script>
  import Results from "$components/Results.svelte";
  import Pokedex from "$components/Pokedex.svelte";
  import { isResultVisibleStore } from "$components/PokemonStore.svelte";
  import { onMount } from "svelte";
  import {
    fetchPokemon,
    fetchPokemonDamageRelations,
    getDamageRelation,
  } from "../../components/Utils.svelte";
  export let params, query;

  // console.log("params ", params);
  // console.log("query ", query);

  let pokemon,
    advantage,
    weakness,
    previousPokemon,
    nextPokemon,
    doubleDamageTo,
    doubleDamageFrom;

  // initial props

  // $: console.log("stats: ", pokemon?.stats);

  onMount(() => {
    const firstIdPokemon = 1;
    const lastIdPokemon = 893;

    const id = parseInt(query.id);
    const previousId = id === firstIdPokemon ? lastIdPokemon : id - 1;
    const nextId = id === lastIdPokemon ? firstIdPokemon : id + 1;

    const pokemonName = params.pokemonName.toLowerCase();

    (async () => {
      if (query.id && query.types) {
        const types = JSON.parse(query.types);

        [
          pokemon,
          previousPokemon,
          nextPokemon,
          [doubleDamageTo, doubleDamageFrom],
        ] = await Promise.all([
          fetchPokemon(pokemonName),
          fetchPokemon(previousId === 188 ? "skiploom" : previousId),
          fetchPokemon(nextId === 188 ? "skiploom" : nextId),
          fetchPokemonDamageRelations(types),
        ]);

        [advantage, weakness] = getDamageRelation(
          pokemon.types,
          doubleDamageTo,
          doubleDamageFrom
        );
      } else {
        const pokemon = await fetchPokemon(pokemonName);

        [previousPokemon, nextPokemon] = await Promise.all([
          fetchPokemon(previousId === 188 ? "skiploom" : previousId),
          fetchPokemon(nextId === 188 ? "skiploom" : nextId),
        ]);

        [doubleDamageTo, doubleDamageFrom] = await fetchPokemonDamageRelations(
          pokemon.types
        );

        [advantage, weakness] = getDamageRelation(
          pokemon.types,
          doubleDamageTo,
          doubleDamageFrom
        );
      }
    })();
  });

  //initial props

  onMount(() => {
    isResultVisibleStore.set(false);
  });
</script>

<style>
  div {
    max-width: 1200px;
    margin: 0 auto;
    background: white;
    padding: 0 1rem;
    padding-bottom: 10%;
  }

  h1 {
    margin: 0;
    padding: 0;
    display: flex;
    justify-content: center;
    align-items: center;
    padding-top: 1.5rem;
  }
</style>

<svelte:head>
  <link rel="icon" href="/pokemon_icon.svg" />
  <link rel="icon" type="image/png" sizes="16x16" href="/pokemon_icon.svg" />
  <title>{params.pokemonName} 🐤</title>
</svelte:head>

<div>
  <Results />

  <h1>{params.pokemonName.toUpperCase()}</h1>

  <Pokedex {pokemon} {advantage} {weakness} {previousPokemon} {nextPokemon} />
</div>

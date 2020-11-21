<script>
  import PokedexChart from "./PokedexChart.svelte";
  import PokedexImage from "./PokedexImage.svelte";
  import PokedexInfo from "./PokedexInfo.svelte";
  import { POKEMONS_TYPE_COLORS } from "./PokemonType.svelte";

  const POKEMON_DEFAULT = {
    name: "venusaur",
    id: 3,
    types: [
      { type: { name: "grass", url: "https://pokeapi.co/api/v2/type/12/" } },
    ],
  };

  const TYPES_DEFAULT = [
    { name: "grass", url: "https://pokeapi.co/api/v2/type/12/" },
    { name: "grass", url: "https://pokeapi.co/api/v2/type/12/" },
    { name: "grass", url: "https://pokeapi.co/api/v2/type/12/" },
  ];

  export let pokemon = POKEMON_DEFAULT,
    advantage = TYPES_DEFAULT,
    weakness = TYPES_DEFAULT,
    previousPokemon = POKEMON_DEFAULT,
    nextPokemon = POKEMON_DEFAULT;

  const { color } = POKEMONS_TYPE_COLORS.find(
    (element) => element.name === pokemon.types[0].type.name
  );

  $: console.log(advantage, weakness);
</script>

<style>
  div {
    display: flex;
    justify-content: space-around;
    flex-wrap: wrap;
    padding: 1rem;
  }
</style>

<div>
  <PokedexImage name={pokemon.name} id={pokemon.id} />

  <PokedexInfo types={pokemon.types} {advantage} {weakness} />
  <PokedexChart stats={pokemon.stats} {color} />
</div>

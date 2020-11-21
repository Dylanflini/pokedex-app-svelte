<script context="module">
  export function normalizeId(id) {
    if (id < 100 && id < 10000) {
      const x = id.toString();
      return addZeroLeft(x);
    } else {
      return id;
    }

    function addZeroLeft(string) {
      if (string.length < 3) {
        string = "0".concat(string);
        return addZeroLeft(string);
      } else {
        return string;
      }
    }
  }
</script>

<script>
  import Card from "./Card.svelte";
  import PokemonType from "./PokemonType.svelte";

  export let name,
    id,
    types = [];

  let src = "";

  $: {
    src = `https://assets.pokemon.com/assets/cms2/img/pokedex/full/${normalizeId(
      id
    )}.png`;
  }
</script>

<style>
</style>

<Card title={name} {src} alt={name}>
  {#each types as type}
    <PokemonType {type} />
  {/each}
</Card>

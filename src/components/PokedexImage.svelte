<script>
import { normalizeId } from "./Pokemon.svelte";


  export let name, id;
</script>

<style>
  div {
    margin-bottom: 1rem;
    flex: 1 1 400px;
    width: 100%;
    max-width: 600px;
    text-align: center;
  }

  img {
    width: inherit;
    /* margin: 0 auto; */
    background: linear-gradient(to right, #83a4d4, #b6fbff);
    border-radius: 4px;
  }
</style>

<div>
  <img
    src={`https://assets.pokemon.com/assets/cms2/img/pokedex/full/${normalizeId(id)}.png`}
    alt={`Is the pokemon called ${name}`} />
</div>

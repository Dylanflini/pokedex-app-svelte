<script>
  import PokemonType from "./PokemonType.svelte";
  import { formatHeight, formatWeight } from "./Utils.svelte";

  export let types,
    advantage,
    weakness,
    height = "70",
    weight = "30";
</script>

<style>
  section {
    margin-bottom: 1rem;
    flex: 1 1 300px;
    width: 100%;
    max-width: 400px;
    border-radius: 4px;

    background: rgb(83, 166, 210);
    padding: 1rem;
    display: flex;
  }
  div{
    margin-left: 1rem;
  }
</style>

<section>
  <div>
    <h2>Type</h2>

    {#each types as type}
      <PokemonType {type} />
    {/each}

    <h2>Advantage</h2>
    {#each advantage as type}
      <PokemonType {type} isDamageRelation={true} />
    {/each}

    <h2>Weakness</h2>
    {#each weakness as type}
      <PokemonType {type} isDamageRelation={true} />
    {/each}
  </div>
  <div>
    <p><strong> Height: </strong>{formatHeight(height)}</p>
    <p><strong>Weight: </strong>{formatWeight(weight)}</p>
  </div>
</section>

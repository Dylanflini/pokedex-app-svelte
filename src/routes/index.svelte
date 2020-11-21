<script>
  import Footer from "$components/Footer.svelte";
  import Results from "$components/Results.svelte";
  import { isResultVisibleStore } from "../components/PokemonStore.svelte";

  export let isLoading = true;

  isResultVisibleStore.set(true);
</script>

<style>
  div {
    max-width: 1200px;
    margin: 0 auto;
  }
</style>

<svelte:head>
  <link rel="icon" href="/pokemon_icon.svg" />
  <link rel="icon" type="image/png" sizes="16x16" href="/pokemon_icon.svg" />
  <title>Pokedex App 🐤</title>
</svelte:head>

<div>
  <Results />
</div>
<Footer text="© Pokedex App Svelte - " version="- version 1.0" />

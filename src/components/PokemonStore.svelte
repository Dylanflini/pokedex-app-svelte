<script context="module">
  import { writable } from "svelte/store";

  export const pokemonsStore = writable([]);
  export const pokemonsFilterStore = writable([]);
  export const typesStore = writable([]);
  export const typeStore = writable("");
  export const isResultVisibleStore = writable(true);
  export const isLoadingStore = writable(true);
  export const pokemonSearchesStore = writable("");
  export const limitStore = writable(20);

</script>
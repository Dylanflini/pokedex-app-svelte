<script context="module">
  export const INITIAL_VALUE = "select type",
    ALL_TYPE = "all";
</script>

<script>
  export let pokemonSearches,
    options = [],
    type;

  let inputValue = "";

  function handleSubmit(e) {
    e.preventDefault();
    pokemonSearches = inputValue;
  }
</script>

<style>
  form {
    text-align: center;
    display: flex;
  }

  input {
    width: 115px;
    padding: 4px 0;
    height: 20px;
    margin: 0 0.9rem;
  }
  select {
    flex: 1 1 200px;
    padding: 2px 4px;
    margin-left: 8px;
    background-color: lightblue;
    border-radius: 2px;
    border: 0px;
    box-shadow: 0 3px 1px -1px rgba(0, 0, 0, 0.2),
      0 1px 1px 0 rgba(0, 0, 0, 0.14), 0 1px 3px 0 rgba(231, 148, 148, 0.12);
  }
</style>

<form on:submit={handleSubmit}>
  <input bind:value={inputValue} />
  <select bind:value={type}>
    <option value={INITIAL_VALUE}>{INITIAL_VALUE}</option>
    <option value={ALL_TYPE}>{ALL_TYPE}</option>
    {#each options as { name }}
      <option value={name}>{name}</option>
    {/each}
  </select>
</form>

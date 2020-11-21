<script>
  export let color = "#e95858",
    data = [],
    holdValue = true,
    maxValue = 100;

  function getHeight(value, maxValue) {
    return (value / maxValue) * 100;
  }

</script>

<style>
  div {
    height: var(--height);
    background: var(--color);
    flex: 1 1 20px;
    max-width: 40px;
    margin: 0 2px;
  }
  span {
    position: relative;
    font-size: 0.8rem;
    top: -20px;
    text-align: center;
  }
</style>

<!-- {#if holdValue} -->
{#each data as value}
  <div style="--height:{getHeight(value, maxValue)}px; --color:{color}">
    <span>{value}</span>
  </div>
{/each}
<!-- {/if} -->

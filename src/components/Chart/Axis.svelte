<script>
  export let x, y, data, maxValue;

  function accumulateAxisY(maxValue, numberOfAxisY, index = 0, arr = []) {
    if (index > numberOfAxisY) {
      return arr;
    }

    const part = (maxValue / numberOfAxisY) * index;

    arr = [...arr, Math.round(part)];

    return accumulateAxisY(maxValue, numberOfAxisY, index + 1, arr);
  }

  function bottom(value, maxValue) {
    return ((value * 0.94) / maxValue) * 100;
  }

  let dataY = accumulateAxisY(maxValue, data);
</script>

<style>
  .axis-y,
  .axis-x {
    font-size: 0.7rem;
    color: rgba(0, 0, 0, 0.7);
  }

  .axis-x {
    display: flex;
    justify-content: space-around;
    padding: 0.3rem 0;
  }

  .axis-y {
    display: flex;
    width: 1.8rem;
    min-width: min-content;
    align-items: center;
    flex-direction: column;
    position: relative;
  }

  .axis-x-item {
    width: 100%;
    text-align: center;
  }

  .axis-y-item {
    position: absolute;
    margin: 0 0.15rem;
  }
</style>

{#if y}
  <div class:axis-y={true}>
    {#each dataY as element}
      <div
        style="--bottom:{bottom(element, maxValue)}%"
        class:axis-y-item={true}>
        {element}
      </div>
    {/each}
  </div>
{/if}

{#if x}
  <div class:axis-x={true}>
    {#each data as element}
      <div class:axis-x-item={true}>{element}</div>
    {/each}
  </div>
{/if}

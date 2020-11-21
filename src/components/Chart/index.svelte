<script>
  import Axis from "./Axis.svelte";
  import Bars from "./Bars.svelte";

  function isNoValidNumber(value) {
    switch (Math.sign(value)) {
      case 1:
        return false;
      case -1:
        return true;
      case 0:
        return true;
      case -0:
        return true;
      default:
        return true;
    }
  }

  export let axisY = 4,
    axisX,
    data,
    maxValue = 400,
    holdValue = true,
    height = "300px",
    barColor = "lightblue";
</script>

<style>
  div {
    width: 100%;
    height: var(--height);
    padding-top: 2rem;
    display: grid;
    grid-template-columns: min-content auto;
    grid-template-rows: auto min-content;
    gap: 0.5vw;
    background-color: #f0f0f0;
    border-radius: 4px;
    box-shadow: 0 3px 1px -1px rgba(0, 0, 0, 0.2),
      0 1px 1px 0 rgba(0, 0, 0, 0.14), 0 1px 3px 0 rgba(231, 148, 148, 0.12);
  }
</style>

<div style="height:{height}">
  <!-- {#if isNoValidNumber(axisY)} -->
    <span ></span>
  <!-- {:else} -->
    <!-- <Axis y={true} data={axisY} {maxValue} /> -->
  <!-- {/if} -->
  <Bars color={barColor} {holdValue} {data} {maxValue} />
  <span />
  <Axis x={true} data={axisX} {maxValue} />
</div>

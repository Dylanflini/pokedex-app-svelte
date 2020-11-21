<script>
  import Chart from "./Chart";
  import { formatData } from "./Utils.svelte";

  export let stats = [],
    color, data, axisX;

  $: {
    [data, axisX] = formatData(stats);
  }

  $: console.log("stas 2: ", stats);
  $: console.log("stats: ", data, axisX);
</script>

<style>
  div {
    margin-bottom: 1rem;
    flex: 1 1 600px;
    width: 100%;
    /* max-width: 600px; */
    text-align: center;

    align-self: flex-end;
    align-content: stretch;
    max-width: 600px;
  }
</style>

<div>
  <h2>Stats</h2>

  <Chart
    height="250px"
    barColor={color}
    axisY={-1}
    {axisX}
    {data}
    maxValue={Math.max(...data)} />
</div>

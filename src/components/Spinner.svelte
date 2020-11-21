<script>
	export let width = "100px"; 
</script>

<style>
  .container {
    position: fixed;
    top: 50%;
    z-index: 1000;
    height: var(--width);
    width: var(--width);
    left: calc(50% - (var(--width) / 2));
  }

  .container div {
    width: 100%;
    height: 100%;

    border: solid 7px #cfd0d1;
    border-bottom-color: red;
    border-radius: 50%;
    will-change: transform;

    animation: spinner 1.2s cubic-bezier(0.58, 0.84, 0.7, 0.45) infinite;
  }

  @keyframes spinner {
    0% {
      transform: rotate(0deg);
    }
    100% {
      transform: rotate(360deg);
    }
  }
</style>

<div class:container={true} style="--width:{width};" ><div /></div>

<script>
  export let text, version;
</script>

<style>
  div {
    display: flex;
    justify-content: center;
    background-color: rgb(224, 0, 48);
    position: fixed;
    bottom: 0;
    width: 100%;
  }
  p {
    color: white;
    margin: 0;
    padding: 10px;
  }
</style>

<div>
  <p>{text}{new Date().getFullYear()} {version}</p>
</div>

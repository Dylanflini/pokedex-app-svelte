<script>
  export let title, src, alt;
</script>

<style>
  .container {
    margin: 0 auto;
    padding: 10px;
    background-color: #dfdbdb;
    font-size: 16px;
    border-radius: 4px;
    color: black;
    font-weight: bold;
    box-shadow: 0 3px 1px - 1px rgba(0, 0, 0, 0.2),
      0 1px 1px 0 rgba(0, 0, 0, 0.14), 0 1px 3px 0 rgba(231, 148, 148, 0.12);
  }
  .container:hover {
    transform: translateY(-2px);
  }
  .description {
    font-size: 12px;
    margin: 0;
    padding: 0;
    width: 100%;
    display: flex;
    height: max-content;
  }
  p {
    font-size: 16px;
    margin: 0;
    padding: 0;
    margin-bottom: 4px;
  }
  img {
    width: 100%;
    height: auto;
  }
</style>

<div class:container={true}>
  <img {src} {alt} />
  <p>{title}</p>
  <div class:description={true}>
    <slot />
  </div>
</div>

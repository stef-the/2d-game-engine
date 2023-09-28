<div id="content">
  <slot />
</div>

<style>
  :global(body) {
    overflow: hidden;
    font-family: "Consolas", sans-serif;
  }

  #content {
    position: fixed;
    top: 0;
    left: 0;
    height: 100vh;
    width: 100vw;
    overflow: hidden;
  }
</style>

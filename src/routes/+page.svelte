<script>
  import { controls, world, c } from "../lib/data.js";
  import { onMount } from "svelte";

  // Set player position
  let pos = {
    x: 0,
    y: 0,
  };

  // Set screen ratio (Allows for responsive design)
  let screenratio = 50;

  // Store value for keys pressed
  let keysDown = [];

  // Handle game updates
  function update() {
    // Handle player movement
    if (keysDown.includes(controls.moveUp)) {
      pos.y -= 1;
    }
    if (keysDown.includes(controls.moveDown)) {
      pos.y += 1;
    }
    if (keysDown.includes(controls.moveLeft)) {
      pos.x -= 1;
    }
    if (keysDown.includes(controls.moveRight)) {
      pos.x += 1;
    }
  }

  // Repeat function asynchronously every interval
  function repeatAsync(func, interval) {
    func();
    setTimeout(() => {
      repeatAsync(func, interval);
    }, interval);
  }

  // Convert js variables to css variables (Allows for responsive design)
  function cssVariables(node, variables) {
    for (const name in variables) {
      node.style.setProperty(`--${name}`, variables[name]);
    }
    return {
      update(variables) {
        for (const name in variables) {
          node.style.setProperty(`--${name}`, variables[name]);
        }
      },
    };
  }

  // Start game loop on mount
  onMount(() => {
    console.log("onMount successful");
    repeatAsync(update, 10);
    console.info("Game loop started");
  });
</script>

<!-- Handle keyboard inputs, set css variables -->
<svelte:body
  on:keydown={(e) => {
    // Handles on key down event
    if (!keysDown.includes(e.key)) {
      keysDown.push(e.key);
    }
  }}
  on:keyup={(e) => {
    // Handles on key up event
    keysDown.splice(keysDown.indexOf(e.key), 1);
  }}
  use:cssVariables={{ screenratio }}
/>

<!-- Render world -->
{#each world as world_column}
  {#each world_column as world_element}
    <div
      class="block"
      id=""
      style="
        background-color: {world_element};
        top: {world.indexOf(world_column) * screenratio -
        pos.y * (screenratio / 20)}px;
        left: {world_column.indexOf(world_element) * screenratio -
        pos.x * (screenratio / 20)}px;"
    />
  {/each}
{/each}

<style>
  .block {
    position: absolute;
    width: calc(var(--screenratio) * 1px);
    height: calc(var(--screenratio) * 1px);
  }
</style>

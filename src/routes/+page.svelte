<script>
  import { controls, world, c } from "../lib/data.js";
  import { onMount, tick } from "svelte";

  // Set player position
  let pos = {
    x: 0,
    y: 0,
  };

  // Set screen ratio (Allows for responsive design)
  let screenratio = 50;

  // Set tickrate
  const tickrate = 10;

  // Store and process value for keys pressed
  let keysDown = [];

  function keyDownHandler(e) {
    if (!keysDown.includes(e)) {
      keysDown.push(e);
    }
  }
  function keyUpHandler(e) {
    keysDown.splice(keysDown.indexOf(e), 1);
  }

  // Store and process value for buttons pressed
  let buttonsOver = [];

  function mouseOverBtn(e) {
    if (!buttonsOver.includes(e.target.id)) {
      buttonsOver.push(e.target.id);
    }
  }
  function mouseOutBtn(e) {
    buttonsOver.splice(buttonsOver.indexOf(e.target.id), 1);
  }

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
    repeatAsync(update, tickrate);
    console.info("Game loop started");
  });
</script>

<!-- Handle keyboard inputs, set css variables -->
<svelte:body
  on:keydown={(e) => {
    // Handles on key down event
    keyDownHandler(e.key);
  }}
  on:keyup={(e) => {
    // Handles on key up event
    keyUpHandler(e.key);
  }}
  on:mousedown={() => {
    // Handles on click event
    for (const button of buttonsOver) {
      switch (button) {
        case "upBtn":
          keyDownHandler(controls.moveUp);
          break;
        case "downBtn":
          keyDownHandler(controls.moveDown);
          break;
        case "leftBtn":
          keyDownHandler(controls.moveLeft);
          break;
        case "rightBtn":
          keyDownHandler(controls.moveRight);
          break;
        case "centerBtn":
          break;
      }
    }
  }}
  on:mouseup={() => {
    for (const button of buttonsOver) {
      switch (button) {
        case "upBtn":
          keyUpHandler(controls.moveUp);
          break;
        case "downBtn":
          keyUpHandler(controls.moveDown);
          break;
        case "leftBtn":
          keyUpHandler(controls.moveLeft);
          break;
        case "rightBtn":
          keyUpHandler(controls.moveRight);
          break;
        case "centerBtn":
          break;
      }
    }
  }}
  use:cssVariables={{ screenratio }}
/>

<div class="buttonContainer">
  <div id="directionalButtons" class="buttonContainer">
    <button
      id="upBtn"
      on:mouseover={(e) => mouseOverBtn(e)}
      on:focus={(e) => mouseOverBtn(e)}
      on:mouseout={(e) => mouseOutBtn(e)}
      on:blur={(e) => mouseOutBtn(e)}>Up</button
    >
    <button
      id="downBtn"
      on:mouseover={(e) => mouseOverBtn(e)}
      on:focus={(e) => mouseOverBtn(e)}
      on:mouseout={(e) => mouseOutBtn(e)}
      on:blur={(e) => mouseOutBtn(e)}>Down</button
    >
    <button
      id="leftBtn"
      on:mouseover={(e) => mouseOverBtn(e)}
      on:focus={(e) => mouseOverBtn(e)}
      on:mouseout={(e) => mouseOutBtn(e)}
      on:blur={(e) => mouseOutBtn(e)}>Left</button
    >
    <button
      id="rightBtn"
      on:mouseover={(e) => mouseOverBtn(e)}
      on:focus={(e) => mouseOverBtn(e)}
      on:mouseout={(e) => mouseOutBtn(e)}
      on:blur={(e) => mouseOutBtn(e)}>Right</button
    >
    <button
      id="centerBtn"
      on:mouseover={(e) => mouseOverBtn(e)}
      on:focus={(e) => mouseOverBtn(e)}
      on:mouseout={(e) => mouseOutBtn(e)}
      on:blur={(e) => mouseOutBtn(e)}>Center</button
    >
  </div>
</div>

<div id="player" />

<!-- Render world -->
<div
  id="world"
  style="top: {pos.y * (screenratio / -20)}px; left: {pos.x *
    (screenratio / -20)}px;"
>
  {#each world as world_column}
    {#each world_column as world_element}
      <div
        class="block"
        style="
        background-color: {world_element};
        top: {world.indexOf(world_column) * screenratio}px;
        left: {world_column.indexOf(world_element) * screenratio}px;"
      />
    {/each}
  {/each}
</div>

<style>
  #world {
    position: fixed;
  }
  .block {
    position: absolute;
    width: calc(var(--screenratio) * 1px);
    height: calc(var(--screenratio) * 1px);
  }

  .buttonContainer button {
    position: fixed;
    height: 5vw;
    width: 5vw;
  }

  #upBtn {
    bottom: 12vw;
    left: 6.5vw;
  }
  #downBtn {
    bottom: 1vw;
    left: 6.5vw;
  }
  #leftBtn {
    bottom: 6.5vw;
    left: 1vw;
  }
  #rightBtn {
    bottom: 6.5vw;
    left: 12vw;
  }
  #centerBtn {
    bottom: 6.5vw;
    left: 6.5vw;
  }
</style>

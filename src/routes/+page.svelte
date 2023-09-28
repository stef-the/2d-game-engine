<script>
  import { controls, world, c } from "../lib/data.js";
  import { onMount } from "svelte";

  // Set player position to 0,0
  let pos = {
    x: 0,
    y: 0,
  };

  /* 
    Set game variables
  */
  let screenratio = 50; // Set screen ratio (Allows for responsive design)
  const debugMenuOpacityDefault = 70; // Set default debug menu opacity
  let debugMenuOpacity = debugMenuOpacityDefault; // Prepare debug menu show and hide

  let lastTick = new Date().getTime(); // Set last tick time
  let lastTickDifference = new Date().getTime() - lastTick; // Set last tick difference
  let lastTickList = []; // Set last tick list
  let avgTickRate = 0; // Set average tick rate
  let totalTicks = 0; // Set total ticks

  const tickrate = 10; // Set update tickrate (ms) -> time in between updates

  let notFrozen = true; // Set freeze state

  function toggleDebugMenu(e) {
    if (e == controls.debugMenu) {
      if (debugMenuOpacity === 0) {
        debugMenuOpacity = debugMenuOpacityDefault;
      } else {
        debugMenuOpacity = 0;
      }
    }
  }

  /*
    Handle interactions with the user
  */
  let keysDown = []; // Store and process value for keys pressed
  let buttonsOver = []; // Store and process value for buttons pressed

  // Handle keys and buttons pressed
  function keyDownHandler(e) {
    if (!keysDown.includes(e)) {
      keysDown.push(e);
    }
  }
  function keyUpHandler(e) {
    keysDown.splice(keysDown.indexOf(e), 1);
  }
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
    lastTickDifference = new Date().getTime() - lastTick; // Calculate last tick difference
    lastTick = new Date().getTime(); // Update last tick time
    lastTickList.push(lastTickDifference); // Add last tick difference to list
    if (totalTicks > 10) {
      lastTickList.shift(); // Remove first item from list
    }
    totalTicks++; // Increment total ticks
    avgTickRate = Math.round(lastTickList.reduce((a, b) => a + b, 0) / lastTickList.length); // Calculate average tick rate
    if (notFrozen) {
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

    // Handles event for single-time actions
    toggleDebugMenu(e.key);
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
  use:cssVariables={{ screenratio, debugMenuOpacity }}
/>

<!-- Render world first -->
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

<!-- Render buttons -->
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

<!-- Render player -->
<div id="player" />

<!-- Render debug info -->
<div id="debug">
  <span>coordinates (x, y): <span>{pos.x}, {pos.y}</span></span><br />
  <span>last tick: {lastTickDifference}ms</span><br />
  <span>total ticks: {totalTicks}</span><br />
  <span>average tick rate: {avgTickRate}ms</span><br />
</div>

<style>
  #debug {
    position: fixed;
    top: 0;
    left: 0;
    background-color: darkgrey;
    color: darkblue;
    opacity: calc(var(--debugMenuOpacity) * 1%);
    font-size: small;
  }

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

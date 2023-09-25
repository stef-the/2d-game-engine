<script>
  let pos = {
    x: 0,
    y: 0,
  };

  // Colour palette
  const c = {
    g0: "#000",
    g1: "#111",
    g2: "#222",
    g3: "#333",
    g4: "#444",
    g5: "#555",
    g6: "#666",
    g7: "#777",
    g8: "#888",
    g9: "#999",
    ga: "#aaa",
    gb: "#bbb",
    gc: "#ccc",
    gd: "#ddd",
    ge: "#eee",
    gf: "#fff",
    r: "#f00",
    o: "#f80",
    y: "#ff0",
    g: "#0f0",
    c: "#0ff",
    b: "#00f",
    p: "#f0f",
  };

  // Create 2D array of world blocks
  const world = [
    [
      c.g0,
      c.g1,
      c.g2,
      c.g3,
      c.g4,
      c.g5,
      c.g6,
      c.g7,
      c.g8,
      c.g9,
      c.ga,
      c.gb,
      c.gc,
      c.gd,
      c.ge,
      c.gf,
    ],
    ["cyan", "magenta", "yellow"],
    ["gray", "purple", "pink"],
  ];
</script>

<!-- Handle keyboard inputs -->
<svelte:body
  on:keydown={(e) => {
    if (e.key === "ArrowUp") pos.y--;
    if (e.key === "ArrowDown") pos.y++;
    if (e.key === "ArrowLeft") pos.x--;
    if (e.key === "ArrowRight") pos.x++;
  }}
/>


<!-- Render world -->
{#each world as world_column}
  {#each world_column as world_element}
    <div
      class="block"
      id=""
      style="
        background-color: {world_element};
        top: {world.indexOf(world_column) * 100 - pos.y * 10}px;
        left: {world_column.indexOf(world_element) * 100 - pos.x * 10}px;"
    />
  {/each}
{/each}

<style>
  .block {
    position: absolute;
    width: 100px;
    height: 100px;
  }
</style>

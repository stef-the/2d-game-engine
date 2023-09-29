
# Contributing

Feel free to fork the project and make your own changes. If you want to contribute to the original project, make a pull request! You can also get in contact with me via discord at **s_tef**.

# Controls

You can set custom controls in `/src/lib/data.js` inside the controls variable (below).

```js
export const controls = { moveUp: "w" }
```

The default controls are as follows:

| **Action** | **Key** |
|-----|-----|
| Move Forwards | `w` |
| Move Backwards | `s` |
| Move Left | `a` |
| Move Right | `d` |
| Open/Close Debug Menu | `m` |
| Toggle Freeze State | `f` |

# Modifying the world

The entire world render (the thing that moves, the background) is within `/src/routes/+page.svelte`, under the div with the id of `world` (below).

```html
<div id="world" style="...">...</div>
```

The world pans around based off of the coordinates in the `pos` variable, declared at the top of `/src/routes/+page.svelte`, and every element within the `world` div has a fixed position, defined using css coordinates with *top* and *left*, using **px** as a unit. You can insert any html tag within the `world` div, and using `position: fixed;` and custom *top* and *left* values, position it within your world. 

There is no render distance yet, so all offscreen elements are rendered at the moment. This is a planned feature.

# Game engine

## Updates / Ticks

The game runs the `update()` function continuously with a 10ms gap in between callbacks, to preserve system resources. You can see the average tick rate amongst other information in the debug menu, and the `tickrate` is declared at the beginning of `/src/routes/+page.svelte`, just after the `pos` variable.

#### Freeze State

There is a toggleable freeze state (controls.freeze, `f` by default), which will stop all processes within the `update()` function apart from tickrate measurement.

THIS DOES NOT STOP ALL GAME UPDATES.

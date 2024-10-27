# julia-set-generator
 
A random Julia set generator, powered by [`create-svelte`](https://github.com/sveltejs/kit/tree/main/packages/create-svelte).

## How it Works
The JavaScript snippet that generates the fractal will be displayed down here with comments explaining how it works.

```javascript
// Importing the onMount lifecycle function from Svelte
import { onMount } from 'svelte';

// Declaring variables
let canvas; // Reference to the canvas element for drawing the fractal
let zoom = 1; // Initial zoom level for the fractal visualization
let maxIterations, cx, cy; // Variables for maximum iterations and complex constants
let equation = ''; // Variable to hold the equation representation for display

// Function to set random values for the fractal parameters
const setRandomValues = () => {
  maxIterations = Math.floor(Math.random() * 100) + 150; // Max iterations between 150 and 250
  cx = Math.random() * 2 - 1; // Random complex constant cx between -1 and 1
  cy = Math.random() * 2 - 1; // Random complex constant cy between -1 and 1
};

// Function to draw the fractal on the canvas
const drawFractal = () => {
  const ctx = canvas.getContext('2d'); // Get the 2D rendering context for the canvas
  const width = canvas.width; // Width of the canvas
  const height = canvas.height; // Height of the canvas
  ctx.clearRect(0, 0, width, height); // Clear the canvas for redrawing

  // Generate new random parameters for each fractal
  setRandomValues(); // Set new random values for maxIterations, cx, and cy
  equation = `f(z) = z^2 + ${cx.toFixed(2)} + i * ${cy.toFixed(2)}`; // Update equation display with new constants

  // Loop through each pixel of the canvas to calculate fractal colors
  for (let x = 0; x < width; x++) {
    for (let y = 0; y < height; y++) {
      // Normalize pixel coordinates to complex plane coordinates
      let zx = (x - width / 2) / (zoom * 100); // X-coordinate in the complex plane
      let zy = (y - height / 2) / (zoom * 100); // Y-coordinate in the complex plane
      let iter = 0; // Iteration counter

      // Iterate to determine if the point escapes the fractal
      while (zx * zx + zy * zy < 4 && iter < maxIterations) {
        let tmp = zx * zx - zy * zy + cx; // Update zx using the fractal equation
        zy = 2.0 * zx * zy + cy; // Update zy using the fractal equation
        zx = tmp; // Assign updated zx value
        iter++; // Increment iteration count
      }

      // Color mapping based on iteration count
      let color; // Variable to store color value for the pixel
      if (iter === maxIterations) {
        color = 'rgb(0, 0, 0)'; // Points inside the set are colored black
      } else {
        // Calculate hue based on the number of iterations and map it to HSL color
        const hue = Math.floor(360 * (iter / maxIterations)); // Map iterations to hue
        color = `hsl(${hue}, 100%, 50%)`; // Assign a bright color in HSL format
      }
      ctx.fillStyle = color; // Set the fill color for the current pixel
      ctx.fillRect(x, y, 1, 1); // Draw the pixel on the canvas
    }
  }
};

// Lifecycle hook to draw the fractal when the component is mounted
onMount(() => {
  drawFractal(); // Call the function to draw the fractal
});
```
##  Examples
This is a list of fractals and the equations corresponding to them (And my nickname of it).

<div align=center>



![Screenshot 2024-10-27 3 29 09 PM](https://github.com/user-attachments/assets/5995c346-262c-423f-a256-7a29a8f9360d)

##### Kite

$$
f(z) = z^2 - 0.06 + i \cdot 0.77
$$
</div>


## Building

To create a production version of your app:

```bash
npm run build
```

You can preview the production build with `npm run preview`.

> To deploy your app, you may need to install an [adapter](https://svelte.dev/docs/kit/adapters) for your target environment.

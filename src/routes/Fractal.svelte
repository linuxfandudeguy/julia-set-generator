<script>
  import { onMount } from 'svelte';
  let canvas;
  let zoom = 1; // Initial zoom level
  let maxIterations, cx, cy;
  let equation = ''; // Variable to hold the equation

  const setRandomValues = () => {
    maxIterations = Math.floor(Math.random() * 100) + 150; // Max iterations between 150 and 250
    cx = Math.random() * 2 - 1;                    // Constant cx between -1 and 1
    cy = Math.random() * 2 - 1;                    // Constant cy between -1 and 1
  };

  const drawFractal = () => {
    const ctx = canvas.getContext('2d');
    const width = canvas.width;
    const height = canvas.height;
    ctx.clearRect(0, 0, width, height);

    // Generate new random parameters for each fractal
    setRandomValues();
    equation = `f(z) = z^2 + ${cx.toFixed(2)} + i * ${cy.toFixed(2)}`; // Update equation display

    for (let x = 0; x < width; x++) {
      for (let y = 0; y < height; y++) {
        let zx = (x - width / 2) / (zoom * 100);
        let zy = (y - height / 2) / (zoom * 100);
        let iter = 0;

        while (zx * zx + zy * zy < 4 && iter < maxIterations) {
          let tmp = zx * zx - zy * zy + cx;
          zy = 2.0 * zx * zy + cy;
          zx = tmp;
          iter++;
        }

        // Color mapping based on iteration count
        let color;
        if (iter === maxIterations) {
          color = 'rgb(0, 0, 0)'; // Inside the set (black)
        } else {
          const hue = Math.floor(360 * (iter / maxIterations));
          color = `hsl(${hue}, 100%, 50%)`; // Bright colors in HSL
        }
        ctx.fillStyle = color;
        ctx.fillRect(x, y, 1, 1);
      }
    }
  };

  onMount(() => {
    drawFractal();
  });
</script>

<div class="flex flex-col items-center space-y-4">
  <button on:click={drawFractal} class="px-4 py-2 bg-blue-600 text-white rounded-md hover:bg-blue-700">
    Generate Julia Set
  </button>
  <canvas bind:this={canvas} width="800" height="800" class="border-2 border-gray-700 rounded-lg"></canvas>
  <div class="equation text-lg font-bold">{equation}</div> <!-- Display the equation -->
</div>

<style>
button {
    font-family: 'Inter', sans-serif;
    font-size: 1rem;
}

canvas {
    margin-top: 1rem;
    border: none; /* Remove border from canvas */
}

.equation {
    margin-top: 1rem;
    color: #000000;
}

body {
    background-color: white;
}
</style>

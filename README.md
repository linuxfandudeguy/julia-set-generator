# julia-set-generator
 
A random Julia set generator, powered by [`create-svelte`](https://github.com/sveltejs/kit/tree/main/packages/create-svelte).

## How it works
```svelte
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
```
## Developing

Once you've created a project and installed dependencies with `npm install` (or `pnpm install` or `yarn`), start a development server:

```bash
npm run dev

# or start the server and open the app in a new browser tab
npm run dev -- --open
```

## Building

To create a production version of your app:

```bash
npm run build
```

You can preview the production build with `npm run preview`.

> To deploy your app, you may need to install an [adapter](https://svelte.dev/docs/kit/adapters) for your target environment.

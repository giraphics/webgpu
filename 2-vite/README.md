# Configuring WebGPU

### Things to be covered in this tutorial:

- Setup with Vite

## Setup with Vite

Here's a step-by-step guide to create a WebGPU project using Vite:

1. **Install Vite globally**: If you haven't already installed Vite, you can do so by running the following command in your terminal:

   ```
   npm install -g vite
   ```

2. **Create a new Vite project**: Navigate to your desired project directory and run the following command:

   ```
   npm init vite@latest
   ```

   This command will guide you through creating a new Vite project.

3. **Configure the project settings**: When prompted, provide the project name. In this case, enter `clear-background`. Choose the project template, which in this case should be "Vanilla" and "Typescript".

4. **Navigate into the project directory**: Once the project has been created, navigate into the project directory:

   ```
   cd clear-background
   ```

5. **Install dependencies**: Run the following command to install the project dependencies:

   ```
   npm install
   ```

6. **Run the development server**: Start the development server by running:

   ```
   npm run dev
   ```

7. **Remove default contents**: From the default project, remove  $\color{red}\text{counter.ts}$, $\color{red}\text{style.css}$, $\color{red}\text{typescript.svg}$.

8. **Edit**: 

   1. From the default project, edit  $\color{red}\text{index.html}$​ remove:

      ```html
      <div id="app"><div>
      ```

      

   2. and add add html canvas:

      ```html
          <canvas id="canvas"
            width="800px" 
            height="600px" 
            style="border: black; border-width: 1px; border-style: solid;">
          </canvas>
      ```

      

    3. Edit  $\color{red}\text{main.ts}$.

      ```ts
   const canvas = document.getElementById("canvas") as HTMLCanvasElement;
   
   async function main() {
       if (!navigator.gpu) {
           throw new Error("WebGPU not supported on this browser.");
       }
   
       const adapter = await navigator.gpu.requestAdapter();
       if (!adapter) {
           throw new Error("No appropriate GPUAdapter found.");
       }
   
       const device = await adapter.requestDevice();
   
       const context = canvas.getContext("webgpu");
       if (!context) return;
   
       const canvasFormat = navigator.gpu.getPreferredCanvasFormat();
       context.configure({
           device: device,
           format: canvasFormat,
       });
   
       const encoder = device.createCommandEncoder();
       const pass = encoder.beginRenderPass({
           colorAttachments: [{
               view: context.getCurrentTexture().createView(),
               loadOp: "clear",
               clearValue: { r: 0, g: 0.4, b: 0.4, a: 1.0 },
               storeOp: "store",
           }]
       });
   
       pass.end();
       device.queue.submit([encoder.finish()]);
   }
   
   main();
      ```

9. **Adding support WebGPU types**: 

   1. Install WebGPU type using`npm install --save @webgpu/types`

   2. In `tsconfig.json`:

      ```
      {
        // ...
        "compilerOptions": {
          // ...
          "types": ["@webgpu/types"]
        }
      }
      ```

      Or you can use `typeRoots`:

      ```
      {
        // ...
        "compilerOptions": {
          // ...
          "typeRoots": ["./node_modules/@webgpu/types", "./node_modules/@types"]
        }
      }
      ```

10. **Run the development server again**:

   ```
   npm run dev
   ```

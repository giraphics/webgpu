---
title: WebGPU Setup
author: Parminder Singh
version: v0.1
date: 05/05/2024
layout: post
header: WebGPU Setup
output:
  pdf_document:
    toc: true
    toc_depth: 2
    highlight: tango
---

# Configuring WebGPU

### Things to be covered in this tutorial:

- Introduction to WebGPU and its Overview
- Vanilla HTML

## Overview of WebGPU

WebGPU is a modern graphics and compute API designed to provide high-performance, low-level access to GPU (Graphics Processing Unit) functionality across multiple platforms, including desktop and mobile devices. It's developed as a successor to WebGL and OpenGL, aiming to provide a more efficient and modern approach to GPU programming.

Here's an overview of some key aspects of WebGPU:

1. **Low-Level API**: WebGPU offers a low-level interface, allowing developers fine-grained control over GPU resources and execution. This level of control enables more efficient use of GPU hardware and better performance optimization.
2. **Modern Features**: WebGPU exposes modern GPU features, including compute shaders, asynchronous compute, and explicit multi-threading. These features enable developers to leverage the full capabilities of modern GPUs for graphics rendering and general-purpose computation tasks.
3. **Cross-Platform Support**: One of the goals of WebGPU is to provide a unified API that works across different platforms and devices, including desktops, laptops, mobile devices, and VR/AR headsets. This allows developers to write GPU-accelerated applications that can run on a wide range of hardware configurations.
4. **Integration with Web Technologies**: WebGPU is designed to integrate seamlessly with existing web technologies, such as HTML, CSS, and JavaScript. It's built on top of the WebGPU Shading Language (WGSL), a language similar to HLSL and GLSL, making it familiar to graphics programmers.
5. **Performance**: By providing low-level access to GPU hardware, WebGPU enables developers to optimize performance for their specific use cases. This can lead to significant performance improvements compared to higher-level graphics APIs.
6. **Safety and Security**: WebGPU is designed with safety and security in mind, providing features such as resource validation and memory safety to prevent common GPU programming errors and security vulnerabilities.
7. **Adoption and Future Outlook**: WebGPU is still in development and has not yet been widely adopted in production applications. However, it is gaining traction among developers and browser vendors, with support for WebGPU being implemented in major web browsers such as Chrome, Firefox, and Safari.

Overall, WebGPU promises to be a powerful tool for developing high-performance graphics and compute applications for the web, with the potential to unlock new possibilities in areas such as gaming, virtual reality, augmented reality, scientific computing, and more.

## Simple HTML

Create  $\color{red}\text{clear-background.html}$ and paste the following code.

```html
<!doctype html>

<html>
  <head>
    <meta charset="utf-8">
    <title>01: Canvas Setup - WebGPU Life</title>
  </head>
  <body>
    <canvas width="512" height="512"></canvas>
    <script type="module">
      const canvas = document.querySelector("canvas");

      // WebGPU device initialization
      if (!navigator.gpu) {
        throw new Error("WebGPU not supported on this browser.");
        console.log('1');
      }

      const adapter = await navigator.gpu.requestAdapter();
      if (!adapter) {
        throw new Error("No appropriate GPUAdapter found.");
      }

      const device = await adapter.requestDevice();

      // Canvas configuration
      const context = canvas.getContext("webgpu");
      const canvasFormat = navigator.gpu.getPreferredCanvasFormat();
      context.configure({
          device: device,
          format: canvasFormat,
      });

      // Clear the canvas with a render pass
      const encoder = device.createCommandEncoder();

      const pass = encoder.beginRenderPass({
          colorAttachments: [{
                  view: context.getCurrentTexture().createView(),
                  loadOp: "clear",
                  clearValue: { r: 0, g: 0, b: 0.4, a: 1.0 },
                  storeOp: "store",
              }]
      });

      pass.end();

      device.queue.submit([encoder.finish()]);
    </script>
  </body>
</html>
```

Now let's break down each step:

1. **Get a reference to the canvas element**: We select the canvas element using `document.querySelector("canvas")`.
2. **Check if WebGPU is supported**: We check if `navigator.gpu` exists, indicating support for WebGPU in the browser. If not, we throw an error.
3. **Initialize WebGPU device**: We use `navigator.gpu.requestAdapter()` to get the GPU adapter and `adapter.requestDevice()` to get the GPU device.
4. **Configure the canvas for WebGPU rendering**: We get the WebGPU rendering context from the canvas using `canvas.getContext("webgpu")`. Then we configure the context with the device and preferred canvas format obtained from the GPU.
5. **Clear the canvas with a render pass**: We create a command encoder, begin a render pass, and specify color attachments for clearing the canvas.
6. **Call the initialization function**: We call the `initWebGPU` function to start the initialization process. We use `catch(console.error)` to handle any errors that may occur during initialization.

This tutorial sets up a basic WebGPU environment in an HTML file, clearing the canvas with a blue color. You can build upon this foundation to create more complex graphics and compute applications using WebGPU.

---
title: "Shaders - Lit and Unlit"
layout: single
date: 2025-11-10
categories:
  - Game Development
tags:
  - Graphics
use_math: true
---



### Shader
* A small program that runs directly on the GPU (Graphics Processing Unit).
* Its job is to tell the GPU *how* to draw an object, pixel by pixel.
* It takes inputs like textures, 3D model data, and scene lighting.
* It outputs the final color for each individual pixel on the object.

---

### Lit Shaders
* **Reacts to light:** They use the scene's lights (directional, point, spot lights) in their calculations.
* **Has shadows:** They can cast and receive shadows.
* **Has highlights:** They can show specular reflections from light sources.
* **Appearance:** Realistic, dynamic, and looks "3D" because it's integrated into the scene's lighting.
* **Use Case:** Most 3D objects: walls, floors, characters, props, server racks, etc.

### Unlit Shaders
* **Ignores light:** They do not use any lighting information from the scene.
* **No shadows:** They cannot cast or receive shadows.
* **No highlights:** They have no specular reflections.
* **Appearance:** Flat, bright, and always renders with its base color. It appears to "glow" in the dark because it isn't affected by shadows.
* **Use Case:** UI elements, special effects (fire, magic), status indicators (like an "on" light), and skyboxes.

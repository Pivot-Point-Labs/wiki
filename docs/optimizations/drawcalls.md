---
title: Draw Calls
description: A draw call is a command that tells the GPU to render geometry. Each draw call has CPU overhead.
#icon: material/file-cog-outline
---

# Draw Calls

## What are draw calls?

A draw call is a command that tells the GPU to render geometry. Each draw call has CPU overhead, so too many draw calls can reduce performance, especially in complex scenes.

In Assetto Corsa, draw calls are influenced by factors such as mesh organization, materials, shaders, and rendering passes. **The number of Blender objects is not the same as the number of draw calls.**

## How to reduce draw calls

- **Combine static meshes:** Join objects that are usually visible together and don't need independent movement or visibility. Avoid combining entire areas into huge meshes, as this can reduce visibility culling efficiency. A good rule of thumb is to combine meshes that are within a radius of a big intersection.
- **Reduce material slots:** A mesh with multiple materials may require multiple draw calls. Use fewer materials where possible. Creating a KN5 file will split meshes by material automatically, increasing the load.
- **Use texture atlases:** Combine textures for compatible materials into one atlas. This allows multiple surfaces to share a material and can reduce draw calls.
- **Reuse materials:** Use the same material across objects whenever their shader settings and texture requirements are compatible. Dont create materials like "Lamppost1". Create "Galvanized Steel" and reuse that for many more meshes.
- **Optimize repeated objects:** Join static objects that are always together and link all to a single shared material when appropriate. When exporting to KN5, all meshes are split by material, so joining 80 lamppost objects that each carry their own unique material still costs 80 draw calls. The same 80 objects joined under one shared material cost only 1 draw call.
- **Use LODs:** Simplify distant objects to reduce geometry and rendering costs. Create and show high detail models only when the player is close to them (0-100) and show a lower poly version of the model at a greater distance (101-400). (The numbers provided are examples and depend on the amount of LODs and their detail)
- **Limit transparency:** Transparent materials can require additional rendering work. Avoid unnecessary transparent surfaces and overlapping geometry.

## Common misconceptions

**Fewer polygons do not necessarily mean fewer draw calls.**
:   Geometry complexity and draw call count are separate performance factors.

**Joining objects does not automatically merge materials.**
:   A single mesh can still generate multiple draw calls.

**One material does not mean one draw call.**
:   Separate objects and rendering passes may still require separate calls.

**Fewer objects do not always mean better performance.**
:   Large meshes can reduce the effectiveness of visibility culling.

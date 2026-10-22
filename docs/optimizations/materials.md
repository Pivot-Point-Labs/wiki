---
title: Materials
description: The shader decides how expensive the surface is to render.
#icon: material/file-cog-outline
---

# Materials
 
## What are materials in Assetto Corsa?
 
A material in the KN5 is a shader plus its settings (colors, specular values and so on) and its texture slots. The shader decides how expensive the surface is to render, and the number of different materials decides how much a mesh has to be split up. Exporting to KN5 splits meshes by material, so the material count feeds directly into the [Draw Calls][what-are-draw-calls] count.
 
## How to keep materials cheap
 
- **Use as few unique materials as possible:** Every extra material is another split and another set of state changes for the GPU. Aim for a small library of reusable materials (for example "Asphalt", "Concrete", "Galvanized Steel", "Painted White") instead of one per object.
- **Find and merge duplicates:** Materials that look the same but have different names ("Material.001", "Material.002", "Lamppost1") count as different materials. In Blender you can use the Outliner in "Blender File" view to remap users of duplicates onto a single material. Do this before the export.
- **Choose the cheapest shader that does the job:** Simple opaque shaders are cheaper than shaders with normal maps, multi-texture blending or reflections. Use the extra features only on surfaces where they are noticeable (for example a normal map on the road, not on every distant box).
- **Prefer alpha test over alpha blend:** Cutout materials (fences, leaves) with alpha test are usually cheaper and avoid sorting problems. Alpha blended materials cost more due to overdraw and are best limited to glass and similar surfaces. See also the transparency note in [Draw Calls][what-are-draw-calls].
- **Be careful with layered and blended shaders:** Shaders that blend several textures (terrain, multi-layer materials) are more expensive per pixel. Use them where they matter, such as large visible ground areas, and not everywhere.
- **Watch extra effects from Custom Shaders Patch:** Additional features such as extra reflections or special shading cost performance on every surface that uses them. Enable them only where they make a visible difference.
- **Name materials by what they are:** Names like "Galvanized Steel" make it easy to see when you are about to create a duplicate.

## Common misconceptions
 
**Joining objects merges materials.**
:   It doesn't. A joined mesh still has its materials, and each one can still become its own draw call. See [Draw Calls][what-are-draw-calls].

**More expensive shaders only cost when they are close.**
:   The cost is per pixel on screen, so a big surface far away can still be expensive.

## Related
 
- [Draw Calls][what-are-draw-calls]
- [Textures][why-do-textures-matter]
- [LODs][what-are-lods]
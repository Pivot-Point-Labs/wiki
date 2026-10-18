---
title: Culling
description: Every culled object saves draw calls, vertex processing and pixel shading
#icon: material/file-cog-outline
---

# Culling

## What is culling?
 
Culling means skipping objects that cannot contribute to the final image, so the CPU and GPU never have to draw them. Every culled object saves draw calls, vertex processing and pixel shading.
 
In Assetto Corsa the useful forms of culling are:
 
- **Frustum culling:** Objects completely outside the camera's view are skipped. This works per object, based on its bounding volume.
- **Distance culling:** Objects are hidden beyond a set distance (in practice done with the LOD out distance, see [LODs](lods.md)).
- **Shadow culling:** Objects that don't cast shadows are not rendered into the shadow maps, which saves a second pass over them.
As far as known, Assetto Corsa does **not** do automatic occlusion culling. An object hidden behind a hill or a building is still drawn as long as it is inside the camera's view and distance range. Everything that is not culled automatically has to be handled by how you build the track.
 
## How to help culling
 
- **Split meshes spatially:** Frustum culling only works if an object can actually be outside the view. A single mesh covering the whole track is always visible, so it is always drawn. Group objects by area, but keep the groups big enough to not explode your [draw calls](drawcalls.md).
- **Split long continuous meshes:** Roads, barriers, fences, curbs and walls that run along the entire track should be cut into segments (for example every 100-200 m, depending on the track). Otherwise the whole strip is drawn even when only a small part is visible. (The number is an example and depends on the track layout and density.)
- **Use distance culling for small props:** Cones, signs, small rocks, bolts and similar details are not noticeable from far away. Give them a LOD out distance so they disappear at a reasonable range.
- **Limit shadow casting:** Disable shadow casting on objects that don't need it: small props, flat decals, ground-level details, distant background objects, and anything that is always inside another object's shadow.
- **Delete geometry nobody sees:** Undersides, back faces of walls, building interiors, terrain below roads and anything permanently hidden. It costs vertex work and increases overdraw without adding anything visually.
- **Keep bounding boxes tight:** A single stray vertex far away from the rest of a mesh enlarges its bounding volume. The object then counts as "visible" much more often than it should. Check for loose vertices and leftover geometry before exporting.
- **Don't export what you don't need:** Make sure helper objects, reference meshes and old test geometry are not part of the final KN5.
## Common misconceptions
 
- **Culled objects are completely free.** Not entirely. Tests still cost CPU time, so thousands of tiny objects can be expensive even if most of them are culled. Culling is a balance with [draw calls](drawcalls.md).
- **More and smaller objects are always better for culling.** Only up to a point. Too many small objects increase the CPU overhead and can lose more than they gain.
- **Objects behind other objects are not rendered.** Without occlusion culling they are still rendered, then painted over. Hidden geometry should be removed or avoided manually.
- **Hiding something in Blender hides it in game.** Only if the object is not exported, or its in-game visibility/render properties are set accordingly.

## Related
 
- [Draw Calls](drawcalls.md)
- [LODs](lods.md)
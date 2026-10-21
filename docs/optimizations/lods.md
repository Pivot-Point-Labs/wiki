---
title: LODs
description: Close objects get full detail, distant objects get a simplified version.
#icon: material/file-cog-outline
---

# LODs
 
## What are LODs?
 
LOD stands for "level of detail". Instead of rendering one high detail model at every distance, you create several versions of the same object and only show the one that fits the current distance to the camera. Close objects get full detail, distant objects get a simplified version, and very distant ones can be hidden completely.
 
In Assetto Corsa track creation this is usually done by giving each object a `LOD_IN` and `LOD_OUT` distance (for example in KsEditor). The object is only rendered while the camera is between those two distances.
 
## How to set up LODs
 
- **Make a simplified version of each important object:** Reduce polygon count, remove tiny details, bevels and inner geometry that cannot be seen from a distance. Blender's Decimate modifier is a good start, but check the result by eye and clean it up manually where needed.
- **Assign distance ranges:** Show the high detail model only when the player is close (0-100) and a lower poly version further away (101-400). Use matching values so one version ends where the next one begins. Otherwise you get gaps (nothing visible) or overlaps (both visible). (The numbers are examples and depend on the amount of LODs and their detail.)
- **Let the last LOD disappear:** Small objects should not stay visible forever. Give the lowest LOD an out distance after which the object is hidden. This is also a form of [culling](culling.md).
- **Simplify materials too:** A lower LOD can use fewer materials, or even a single shared one. Together with merged meshes this is what actually saves draw calls.
- **Disable shadows on lower LODs:** Distant shadow casters are rarely noticeable, but still cost rendering time.
- **Keep silhouette and UVs close:** The LOD should look like the original from the distance where it takes over. A big change in shape, color or texture is visible as "popping".
- **Use cheap stand-ins for trees and similar objects:** Far away, a few crossed planes or a very low poly shape with a baked texture can replace a full model.

---

## ext_config example

```ini
; This targets every mesh to be hidden after 300m
[SHADER_REPLACEMENT_...]
MESHES = ?
LOD_IN = 0
LOD_OUT = 300

; This targets any mesh that ends in _LOD1
; This overwrites the block above for selected meshes
[SHADER_REPLACEMENT_...]
MESHES = ?_LOD1
LOD_IN = 0
LOD_OUT = 100

[SHADER_REPLACEMENT_...]
MESHES = ?_LOD2
LOD_IN = 100
LOD_OUT = 300
```

---

## What to put into LODs
 
Good candidates:
 
- Buildings, grandstands and bridges with lots of detail
- Trees, lampposts and other props that are repeated many times
- Barriers and fences with complex shapes
Usually not worth it:
 
- Very low poly objects (nothing to simplify)
- Flat objects such as road surface, markings and decals
- Objects that are only visible from a short range anyway
## Common misconceptions
 
- **LODs automatically reduce draw calls.** They reduce geometry. A lower LOD with the same number of meshes and materials costs about the same number of draw calls as the high one. See [Draw Calls](drawcalls.md).
- **More LOD levels are always better.** Each level adds objects, memory and file size, and more places to cause popping. Two or three levels are enough for most objects.
- **LODs replace good modelling.** If the high detail model is far too heavy, it will still hurt performance wherever it is visible.
- **Everything needs LODs.** For small, simple props the extra objects can cost more than they save.

## Related
 
- [Draw Calls](drawcalls.md)
- [Culling](culling.md)
- [Materials](materials.md)
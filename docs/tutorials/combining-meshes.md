---
title: Combining Meshes
description: A beginner-friendly guide to combining meshes in Blender and Race Track Builder to reduce draw calls.
---

# Combining Meshes

One of the easiest ways to reduce the amount of objects in your scene is by combining meshes that share the same material. This helps with batching and can lower draw calls, which is especially useful for large track scenes.

!!! warning "Don't overdo it"

    Combining everything into one giant mesh will hurt culling (frustum/occlusion). Only combine static objects that you know will stay together and share the same material. Read [draw calls][what-are-draw-calls]{ data-preview } and [culling][what-is-culling]{ data-preview }.

=== "Blender"

    ## Blender

    ### What you'll need

    - Basic knowledge of Blender's Object Mode and Edit Mode

    ### Step 1: Identify what to combine

    Look for static pieces that make sense as one object. Good examples:

    - A modular wall made of multiple separate planks
    - Repeated trim pieces that form one continuous section
    - Scenery props that are always placed as a set and use the same material



    ### Step 2: Select the objects

    <img src="../assets/meshmerge.png" style="width:300px;float:right;margin-left:30px"></img>

    1. Open your scene in Blender
    2. Switch to **Object Mode** (`Tab` if you're in Edit Mode)
    3. Select all meshes you want to join. Hold `Shift` and click each one

    ### Step 3: Join them together

    1. With all objects selected, press `Ctrl + J` on your keyboard
    2. Blender will merge them into one single object

    !!! tip "Tip"

        You can also find this under `Object > Join` in the top bar.

    ### Step 4: Clean up

    !!! info "Info"

        This is only necessary when you join meshes that "touch" each other, like two walls and should be done with care, as it can also break stuff.

    1. Enter **Edit Mode** (`Tab`)
    2. Remove doubles: `M > By Distance` (this merges overlapping vertices)
    3. Recalculate normals if needed: `Shift + N` (especially after joining hard-surface pieces)
    4. Exit Edit Mode (`Tab`)

    ### Important Notes for Blender

    - **Don't join across different materials unnecessarily**: If you plan to merge materials later, that's fine. But joining objects just to "clean up" when they need different materials isn't helpful for draw calls.
    - **Keep collision separate**: Collision meshes (`1WALL`, `AC_PHYS`) should almost always stay as their own object(s).

=== "Race Track Builder"

    ## Race Track Builder (RTB)

    !!! info "Do most of the work in Blender"

        RTB isn't designed for heavy mesh editing. For best results, combine your meshes in Blender first after exporting them from RTB. You will have to do this every time you export a new map or an update, unless you re-import the combined mesh into RTB and use that instead.

    ### Step 1: Prepare your mesh

    Follow the Blender steps above to create a clean, combined mesh. Give it a clear, descriptive name (e.g. `wall_section_a`) to find it easily later.

    ### Step 2: Export & Import

    1. Export your combined mesh in the format RTB expects for your project
    2. Import it into your track like you would any other mesh
    3. Assign the correct material in RTB

    ### Step 3: Place and test

    1. Position the combined object in its final spot
    2. Test visibility from different camera angles (cockpit, chase, etc.)
    3. If it becomes very large, consider splitting it into a couple of smaller cullable sections instead.

    ### Tips for RTB

    - **Cullable chunks > one mega mesh**: Splitting a long building into 2-4 pieces often performs better than merging the entire thing.
    - **Keep modularity in mind**: If you might reuse a piece in multiple places, it's often better to keep it as its own object rather than permanently joining it to something else.
    - **Material match**: For draw call benefits, the combined mesh must share the same material as intended.

    ## Quick Checklist

    - [ ] Only combining **static** geometry
    - [ ] All selected meshes share the **same material** (or you're planning to fix materials next)
    - [ ] Cleaned up doubles & normals after joining
    - [ ] Didn't join instanced/linked duplicates
    - [ ] Kept collision separate
    - [ ] Split into reasonable, cullable chunks if very large

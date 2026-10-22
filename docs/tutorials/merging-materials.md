---
title: Merging Materials
description: A beginner-friendly tutorial on consolidating duplicate materials in Blender and RTB.
icon: lucide/merge
---

# Merging Materials

Having many duplicate materials is a common issue. Things like `Concrete.001`, `Concrete.002`, `Concrete.003` all look the same, but count as different materials. Merging them down to a single shared material is one of the biggest performance wins for draw calls.
.
!!! tip "Goal"
    The goal isn't to use as few materials as possible at all costs. The goal is to **reuse the same material** across objects that share the same surface look.

=== "Blender"

    ## Blender

    ### What you'll need
    - Your track/prop scene open in Blender
    - The textures you're using already assigned

    ### Step 1: Find duplicate materials
    1. Go to the **Material Properties** tab (sphere icon) on the right
    2. Look through your objects and see repeated material names with `.001`, `.002` etc.
    3. Also check the **Outliner > Blender File > Materials** view to see the full list

    ### Step 2: Pick your master material
    Choose one existing material to be the "master". Ideally one that's already correct (right textures, right settings).

    ### Step 3: Reassign objects to the master material
    For each object using a duplicate:

    1. Select the object in **Object Mode**
    2. Go to **Material Properties** panel
    3. Under "Material Slots", remove the duplicate by clicking the `-` button if it's the only wrong one, or just change the slot to use your master material from the dropdown
    4. Repeat for all affected objects

    You can also select multiple objects at once: select them all (`Shift+Click`), make sure your master-material object is active (last selected), then press `Ctrl+L > Materials` to copy materials from active to selected. This is a huge time saver!

    ### Step 4: Clean up unused materials
    1. Go to `File > Clean Up > Unused Data Blocks`
    2. Or switch to the **Orphan Data** view (if available) and purge unused materials
    3. Check your material list - duplicates should be gone

    ### Bonus: Avoid future duplicates
    - When appending/linking assets, pay attention to materials
    - Use **node groups** if you want variation (masks, vertex colors) while keeping the same base material
    - Consider using a **texture atlas** for small props - this lets you put many different decals/signs on one texture and use just one material

=== "Race Track Builder"

    ## Race Track Builder (RTB)

    !!! info "Fix in Blender whenever possible"
        It's far easier and safer to merge materials in Blender before exporting. RTB can reassign materials, but fixing the source file prevents it from happening again.

    !!! note "Still under construction"
        This section is still being written. Check back later.

<!--
    ### Step 1: Import your cleaned-up file
    Export your .blend changes and import/update your meshes in RTB as usual. With unified materials in Blender, you should see far fewer duplicates in RTB.

    ### Step 2: Spot-check in RTB
    1. Open your track in RTB
    2. Look through the materials assigned to your objects
    3. If you still see duplicates (sometimes happens with certain export workflows), you can reassign:

    1. Select the object(s) in RTB
    2. Assign them to the correct shared material instead of the duplicate

    ### Step 3: Test
    Look around your track and make sure nothing lost its look. Pay extra attention to transparent objects (decals, fences) as they need correct alpha settings.
-->
---

## Tips for Beginners

- **Group by look, not by object**: If two objects look identical, they should probably share the same material.
- **Atlases are your friend**: Got 10 different signs? Put them all on one 1024/2048 texture and use one material. This is cleaner than 10 materials.
- **Be careful with transparency**: Opaque and transparent surfaces often need different shader settings. Don't merge a glass material with a concrete material.
- **UVs matter**: If two objects use the same material but their UVs are laid out badly, you'll get texture stretching/bleeding. Fix UVs before merging.
- **Vertex colors can add variation**: Instead of making a new material for "wet" vs "dry" looking concrete, you can use vertex colors + a node setup to tint. This keeps one material.



## Quick Checklist

- [ ] Found all `.001/.002` duplicate materials
- [ ] Chose one clean master material
- [ ] Reassigned all affected objects to it (`Ctrl+L > Materials` is your friend)
- [ ] Cleaned up unused materials
- [ ] Verified looks are unchanged in Blender
- [ ] Spot-checked in RTB after export/import
# Extension Config

The extension config itself is already quite optimized, however there are a few things you can do to increase performance.


## Debugging

To debug your extension config, add this to the top of it and start the game once:

```ini
[__DEBUG]
DUMP_FLATTEN_INI = 1
```

This parses and unwraps the config and shows you how the values are used by the game. This is especially usefull for any advanced setups as mentioned in the [INIpp](https://github.com/ac-custom-shaders-patch/inipp) documentation - like templates, generators, includes, variables, expression, functions and mixins.


## Shader replacements vs Material Adjustments

Shader replacements (`[SHADER_REPLACEMENT_...]`) were originally meant to replace the shaders of existing models. Over time they grew into much more: they can also change material parameters and properties, textures, object parameters, and in some cases even a bit of behaviour.

### What is the difference?

The main difference is **when** the changes are applied:

- **Shader replacements are static.** They are applied once, either while the track loads or after the config is changed. After that they cost **nothing**: no per-frame work and no extra RAM.
- **Material adjustments (`[MATERIAL_ADJUSTMENT_...]` for tracks, `[EMISSIVE_...]` for cars) are dynamic.** They are updated **every single frame**, for as long as the track is loaded. That is continuous CPU work and extra memory use, and every additional adjustment adds to it.

### Which one should I use?

**If you don't need conditions, always use shader replacements.**

A material adjustment that sets the same value every frame is pure waste: you pay the per-frame cost over and over for a result that never changes. Static colors, textures, parameters and other fixed tweaks should never be done with a material adjustment.

Use material adjustments only when a value has to react to something at runtime, such as a condition. If it doesn't, a shader replacement gives the exact same result without any ongoing cost.

### Things to keep in mind

- **Removing a `[SHADER_REPLACEMENT_...]` section does not restore the original values.** Because the changes are applied once, deleting the section from the config will not undo them. Restart the game (or reload the track) to see the original state again.
- **Converting is worth it.** If your config already contains material adjustments without conditions, moving them to shader replacements is a free performance gain.
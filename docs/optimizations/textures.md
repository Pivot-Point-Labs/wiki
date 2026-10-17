# Textures
 
## Why do textures matter?
 
Textures mostly affect **video memory (VRAM)**, memory bandwidth, loading times and the size of the KN5 file. They usually do not change the number of draw calls directly. Too many or too large textures lead to stutters, long loading times, or crashes on cards with little VRAM, especially in combination with many cars on track.
 
## How to reduce texture cost
 
- **Match resolution to importance:** Choose the size based on how large the surface appears on screen, not on how detailed the source is. A bolt or a small sign does not need 2048x2048. Keep a consistent texel density across similar objects, so the track looks uniform.
- **Always prefer DDS with pre-generated mipmaps:** PNG and JPG files contain no mipmaps. They are generated in memory every time the track starts, which increases loading time. If a DDS already contains its mips, the engine/CSP skips that step. PNG and JPG are also decompressed in memory, while DDS with block compression (DXT1 for textures without alpha, DXT5 when you need an alpha channel) uses only a fraction of the VRAM. Mipmaps also make distant surfaces cheaper and cleaner.
- **Use power-of-two sizes:** 256, 512, 1024, 2048 and so on. These work best with mipmaps and compression.
- **Reuse textures:** Use tileable textures (asphalt, concrete, metal, grass) across many objects instead of unique textures per object. This is also what allows shared materials, see [Materials](materials.md).
- **Don't embed the same texture twice:** The same image under different file names or in different folders can end up stored multiple times in the KN5. Keep one file per texture.
- **Use trim sheets and atlases:** Put many small details (signs, barriers, props) on one shared texture. This saves memory and lets objects share a material, which helps [Draw Calls](drawcalls.md).
- **Skip maps you don't need:** A flat, matte surface does not need a normal map or a specular map. Don't keep an alpha channel if nothing uses it. Be careful with shaders that read data from the alpha channel of a texture, since removing it changes the look.
- **Check the final size:** Look at the finished KN5 and the biggest textures in it. A few oversized textures are usually responsible for most of the memory use.
## Common misconceptions
 
- **Bigger textures always look better.** With mipmaps and distance, the extra resolution is often never visible, but you still pay for it in memory.
- **Smaller PNG/JPG files mean less VRAM.** File size on disk and size in memory are two different things.
- **PNG/JPG are fine because the game handles mipmaps itself.** It does, but it has to generate them in memory on every track start, which makes loading slower. DDS with mips included avoids that.
- **Texture size affects draw calls.** It doesn't. The number of materials and meshes does. Sharing textures (atlases) helps draw calls only because materials can be shared.
- **A 4K texture on a small prop is a safe choice.** It is wasted memory. Reuse a shared texture instead.
## Related
 
- [Materials](materials.md)
- [Draw Calls](drawcalls.md)
 
---
title: Texture Creation & Modification (DDS with Mipmaps)
description: A beginner-friendly guide to creating, editing, and exporting DDS textures with proper mipmaps for Assetto Corsa.
---

# Texture Creation & Modification (DDS with Mipmaps)

Assetto Corsa and CSP work best with **.DDS** textures that include **mipmaps**. If you use PNG/JPG, the game has to generate mipmaps on load (slower) and they take more VRAM in practice. This guide walks you through creating clean textures and converting them properly.

!!! info "Keep your source files!"
    Always save your working file (PSD, KRA, or .blend) separately from your game-ready DDS. That way you can easily edit later without redoing everything.

## 1) Plan Your Texture

Before painting, think about how it'll be used in-game.

- **Size matters**: Use power-of-two dimensions: `256`, `512`, `1024`, `2048`, `4096`. These work best with mipmaps.
- **Match visibility**: A tiny bolt doesn't need 4K. A large building facade might benefit from 2K or 4K, but test sensibly.
- **Use atlases**: Put many small details (signs, stickers, props) onto one shared texture. This lets them share one material (great for draw calls).
- **Don't duplicate**: If the same texture is used in multiple places, keep a single file.

## 2) Create or Edit Your Texture

You can use any image editor you're comfortable with:

| Tool | Free | Notes |
|---|---|---|
| [Krita](https://krita.org/) | Yes | Great for hand-painting, very beginner-friendly |
| [Paint.NET](https://www.getpaint.net/) | Yes | Lightweight and easy to learn |
| [GIMP](https://www.gimp.org/) | Yes | Powerful, cross-platform |
| [Photoshop](https://www.adobe.com/photoshop) | Paid | Industry standard |
| [Blender Texture Paint](https://docs.blender.org/manual/en/latest/sculpt_paint/texture_paint/index.html) | Yes | Built-in, perfect if you're already in Blender |

### Tips while creating

- **Work at your target resolution** or a bit higher. Avoid extreme upscaling later.
- **Keep it clean**: Avoid unnecessary noise if it'll be viewed from a distance.
- **Alpha channel**: Only add transparency if something needs to be cut out (e.g., leaves, chainlink, decals). Don't leave unused alpha.
- **Normal maps**: If you bake or create a normal map, keep it linear (don't apply color curves) and preview in your tool.

## 3) Choose the Right DDS Compression

When converting to DDS, picking the right format is important. For most track textures, these are what you'll want:

| Format | When to use | Notes |
|---|---|---|
| **DXT1 (BC1)** | Opaque textures with no transparency | Smallest file size/VRAM. Use for asphalt, concrete, bricks, metal, rock etc. |
| **DXT5 (BC3)** | Textures that use an alpha channel | Needed for cutouts, decals with holes, or anything with transparency. Slightly larger than DXT1. |
| **BC5** | Normal maps | Often preferred for tangent-space normal maps as it preserves detail better. Check what your workflow expects. |

!!! warning "DXT5 on opaque textures is wasteful"
    If your texture has no visible transparency, use **DXT1**. Using DXT5 will use more VRAM for no benefit.

## 4) Convert to DDS with Mipmaps

You need a tool to generate the DDS with mipmap chain. Here are beginner-friendly options:

| Tool | Free | Notes |
|---|---|---|
| [NVIDIA Texture Tools Exporter](https://developer.nvidia.com/nvidia-texture-tools-exporter) | Yes | GUI, very easy. Just open image, pick format, enable mipmaps, save. |
| [TexConv](https://github.com/Microsoft/DirectXTex/wiki/Texconv) | Yes | Command-line. Great for batch converting many textures at once. |
| [nvtt_export](https://github.com/castano/nvidia-texture-tools) | Yes | Modern fork of NVIDIA tools, also good. |

### Option A: NVIDIA Texture Tools Exporter (Recommended for beginners)

1. Download and install [NVIDIA Texture Tools Exporter](https://developer.nvidia.com/nvidia-texture-tools-exporter)
2. Open your exported PNG/TGA image (`File > Open`)
3. Under **Texture Type**, keep as "Color" (or "Normal Map" if it's a normal)
4. Set **Format**:
   - `DXT1` for opaque
   - `DXT5` for alpha/transparency
   - `BC5` for normal maps (if available)
5. Check **Generate Mipmaps** (should be enabled by default)
6. Hit `Save as...` and choose `.DDS`
7. Done!

### Option B: TexConv (Batch-friendly)

If you have a folder of textures to convert, `texconv` is perfect. Open a command prompt in your textures folder.

```bash
# Opaque texture - DXT1 with full mipchain
texconv -f DXT1 -m 0 diffuse.png

# With alpha - DXT5 with full mipchain
texconv -f DXT5 -m 0 decal.png

# Normal map - BC5
texconv -f BC5_UNORM -m 0 normal.png
```

- `-m 0` means "generate mipmaps down to 1x1"
- `-o` lets you specify an output name/path if needed: `-o converted.dds`

## 5) Use in Blender & RTB

### In Blender

1. Import your DDS into Blender's Image Editor, or assign it directly to your material's Image Texture node
2. Keep your node setup simple - just plug your color/normal into the appropriate sockets
3. When exporting for your track, make sure the texture paths stay correct relative to your track files

### In RTB

1. Place your `.DDS` files in your track's texture folder (follow your track's existing structure)
2. Assign them to materials in RTB
3. Avoid mixing duplicate textures with different names - use consistent names
4. Do a quick visual check: distant parts should look smooth without harsh flickering/shimmering

## Tips & Common Mistakes

- **Mipmaps prevent shimmer**: Good mipmaps make distant surfaces look cleaner. Badly generated ones can look blurry - stick to clean source art.
- **Avoid converting JPG**: JPG has compression artifacts. Export a lossless image (PNG, TGA) from your editor, then convert to DDS.
- **Power-of-two is king**: If your image isn't power-of-two, resize it in your image editor first. Non-power-of-two can cause odd behavior.
- **Don't over-compress**: DXT compression is lossy by nature. Keep high-contrast edges clean in your source to minimize visible artifacts.
- **Normal map orientation**: Test your normal map in-game/lookdev. Sometimes green channel needs to be flipped depending on your tools - only change if it looks wrong.
- **Check alpha usage**: If you see gray fringing around cutouts, your alpha might be too soft or the format might be wrong.
- **Preview lower mips**: In NVIDIA Texture Tools, you can often preview mip levels. Quickly scan Mip1/Mip2 for ugly banding.

## Quick Checklist

- [ ] Source saved as `.psd`/`.kra` (kept safe)
- [ ] Exported to lossless `.png` or `.tga` for conversion
- [ ] Resolution is power-of-two
- [ ] Chose correct format (`DXT1`/`DXT5`/`BC5`)
- [ ] Generated mipmaps (`-m 0` or "Generate Mipmaps" enabled)
- [ ] Saved as `.DDS`
- [ ] Placed in correct track texture folder
- [ ] Looks correct in RTB/Blender render
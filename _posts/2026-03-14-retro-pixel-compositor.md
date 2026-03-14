---
layout: post
title: "Retro Pixel Compositor: C64 Palette + Ligne Claire Outlines"
date: 2026-03-14 12:00:00 +0100
categories: [ninjamultirpg, shaders, godot]
---

Today we built a unified retro pixel compositor shader for NinjaMultiRpg, combining C64 palette remapping, Bayer dithering, and ligne claire outlines into a single compute shader pass. The result is a gorgeous pixel art aesthetic that runs entirely in the GPU.

## The Problem

We had two separate effects that needed to work together:
1. **C64 palette remapping + ordered dithering** (canvas_item shader)
2. **Ligne claire outlines** (depth + normal edge detection)

Running them separately caused issues - the order of operations mattered, and the dithering was creating false edges for the outline detection.

## The Solution: Merged Compositor

By combining everything into one compute shader, we got:
- Guaranteed order of operations
- Single pass efficiency
- Ability to skip dithering on edge pixels (crisp outlines!)
- Pre-palette luminance sampling for smart outline colors

### Starting Point: Collision Mesh Issues

Before even getting to shaders, we hit issues with raycasting against detailed cliff geometry. The solution? Separate visual mesh from collision:

![Collision mesh comparison](/assets/images/2026-03-14-retro-pixel-compositor/02-collision-comparison.jpg)
*Blender remesh comparison: left is still too detailed, middle blocky version is perfect for physics*

The blocky collision mesh gives reliable `is_on_floor()` detection and predictable mantle edges.

### First Working Version

Once the compositor was hooked up:

![First working dithering](/assets/images/2026-03-14-retro-pixel-compositor/04-dithering-working.jpg)
*Initial C64 palette with Bayer dithering - the gradients on the cliffs look fantastic*

### The Grazing Angle Problem

Normal-based edge detection went crazy on surfaces viewed at shallow angles:

![Grazing angle issue](/assets/images/2026-03-14-retro-pixel-compositor/05-grazing-angle-issue.jpg)
*The cliff top is drowning in false edge detections*

Fix: Check the surface normal's Z component (in view space) and reduce edge sensitivity at grazing angles:

```glsl
float facing = abs(center_n.z);  // 1.0 = facing camera, 0.0 = grazing
float grazing_factor = smoothstep(0.1, 0.5, facing);
float adjusted_threshold = params.normal_threshold / max(grazing_factor, 0.2);
```

### Color Saturation Control

The C64 palette has vivid colors, but PBR rendering produces muted tones. We added pre-palette saturation and exposure controls:

![Saturation experiments](/assets/images/2026-03-14-retro-pixel-compositor/06-saturation-lava.jpg)
*Boosted saturation pushes colors toward vibrant palette entries*

### Shadow Rim Outlines (Obra Dinn Trick)

The knight characters were getting lost in shadows. Instead of adding more lights, we made outlines in dark areas use a **blue rim color** - faking the look of rim lighting:

![Shadow rim outlines](/assets/images/2026-03-14-retro-pixel-compositor/08-shadow-rim-outlines.jpg)
*Blue outlines in shadows, black outlines in lit areas*

Key insight: Check luminance **before** palette remapping, not after! Otherwise a lit area that maps to a dark palette color would incorrectly get shadow outlines.

### Final Result

![Final result](/assets/images/2026-03-14-retro-pixel-compositor/09-final-result.jpg)
*Pure C64 vibes with the purple/yellow palette, clean dithering, and blue shadow rim outlines on the knights*

## Shader Parameters

The final compositor exposes:

**Outlines:**
- `depth_threshold` / `normal_threshold` - edge sensitivity
- `line_color` - outline color for bright areas
- `shadow_line_color` - rim color for shadows (blue default)
- `shadow_threshold` - luminance cutoff for rim outlines

**Color Processing:**
- `exposure` - brightness pre-palette
- `saturation` - color intensity pre-palette
- `posterize_levels` - optional toon banding (0 = off)

**Dithering:**
- `dither_strength` - Bayer dither intensity
- `skip_dither_on_edges` - keeps outlines pixel-perfect

## What's Next

- Knight remodeling for distinct team colors and swappable helmets
- Testing the effect in multiplayer with 4 differently-colored knights
- Possibly adding an "inner glow" effect for the player character

The kids have been playtesting and already suggested different helmets per player - great instinct for readable multiplayer visuals!

---

*NinjaMultiRpg is a multiplayer action game running on Godot 4.4 with a retro C64 aesthetic.*

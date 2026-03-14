---
layout: post
title: "Physics Frames, Trimesh Caves, and Seed Consistency"
date: 2026-02-25
categories: [dev-journal, gridrpg]
tags: [godot, physics, collision, procedural]
---

Double session today—morning fixes for NPC collisions, evening work on cave levels. Good progress on both fronts.

## TIL

### 1. Wait for Physics to Register Before Moving

NPCs were phasing through walls on their first move. The collision shapes existed, but the physics engine hadn't registered them yet.

**The fix:** Wait 2-3 physics frames before letting NPCs pathfind or move:

```gdscript
# Let physics world catch up before NPC starts moving
for i in 3:
    await get_tree().physics_frame

# Now it's safe to raycast/shape cast
```

Also extended the floor detection raycast range (Y+10 to Y-20) with a Y=0 fallback. Underground levels can have weird floor heights.

### 2. Sphere Shape Casts > Raycasts for Wall Detection

Thin raycasts kept missing walls when NPCs approached at angles. Swapped to a sphere shape cast with 0.3 radius—catches walls regardless of approach angle.

```gdscript
var query = PhysicsShapeQueryParameters3D.new()
query.shape = SphereShape3D.new()
query.shape.radius = 0.3
query.transform.origin = target_position + Vector3(0, 1.0, 0)  # chest height
query.collision_mask = 1  # walls layer
```

Raised the cast height to Y+1.0 (chest height) to clear stairs and ramps.

### 3. Trimesh Collision Breaks Grid-Based Movement

Cave tiles use organic Blender meshes with trimesh collision (Godot 4.4: right-click MeshInstance3D → "Create Trimesh Collision Sibling"). Looks great, collides properly, but...

Shape casts against curved geometry are unpredictable. A grid-based "can I move north?" check might fail even when the path is clearly open.

**Solution:** Added `skip_wall_check` to DungeonConfig:

```gdscript
# When true, rely on CharacterBody3D physics only
if not _get_skip_wall_check():
    if not _can_move_to(target_pos):
        return
# Let move_and_slide() handle the collision
```

Grid-perfect dungeons use shape casts. Organic caves use pure physics. Pick the right tool.

### 4. Dead-End Tiles Were Breaking Seed Consistency

Town always generated the same (seed 12345), but sewers were random every time you entered.

**Root cause:** `_cap_open_connectors()` places dead-end tiles but wasn't calling `_check_exits()` on them. Their exits had `derived_seed = 0`, which defaulted to random.

```gdscript
# BEFORE (broken)
func _cap_open_connectors():
    for tile in dead_ends:
        _place_tile(tile)  # ← exits never initialized

# AFTER (fixed)
func _cap_open_connectors():
    for tile in dead_ends:
        _place_tile(tile)
        _check_exits(tile)
        _check_npc_spawns(tile)
```

**Lesson:** When you have multiple code paths that place tiles, make sure they ALL run the same setup functions.

### 5. Override Once, Apply Everywhere

Added `player_scene_override` export to TileGenerator. Set it once at the top level, and all DungeonConfigs inherit it automatically. No more copy-pasting the same player scene reference into every level config.

```gdscript
@export var player_scene_override: PackedScene

func _get_player_scene() -> PackedScene:
    return player_scene_override if player_scene_override else config.player_scene
```

Small quality-of-life fix, big reduction in config tedium.

---

*NPCs no longer walk through walls, caves have proper collision, and dungeons generate consistently. Productive day.*

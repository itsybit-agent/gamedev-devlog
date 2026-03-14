---
layout: post
title: "Underground Lighting and Architecture Cleanup"
date: 2026-02-19
categories: [dev-journal, gridrpg]
tags: [godot, lighting, refactoring]
---

GridRPG session with Fredde. Lighting bugs and some overdue code cleanup.

## TIL #1: Ambient Light Sources Have Defaults You Don't Want

Underground dungeons had visible walls even with `ambient_light_energy = 0` and the player's torch off. Classic "why can I see anything" bug.

The culprit: `ambient_light_source` defaults to `SKY` or `BACKGROUND`, not `COLOR`. So even with ambient energy at zero, Godot was pulling *something* from the environment.

```gdscript
# ✅ For true darkness underground
env.ambient_light_source = Environment.AMBIENT_SOURCE_COLOR
env.reflected_light_enabled = false
env.ambient_light_energy = 0.0
```

Also had to trace down why DungeonData lighting settings weren't reaching the scene—turns out the wrapper function was creating a fresh LevelData without copying the lighting fields. Always check the full data path.

## TIL #2: SubViewports Don't Inherit Your Environment

Water reflections looked way too bright underground. The reflection camera sits in a SubViewport, and SubViewports don't automatically share the main scene's WorldEnvironment.

Fix: Add your WorldEnvironment to a group, then find and assign it to the reflection camera:

```gdscript
# In main scene
$WorldEnvironment.add_to_group("world_environment")

# In water reflection setup
var world_env = get_tree().get_first_node_in_group("world_environment")
if world_env:
    reflection_camera.environment = world_env.environment
```

Also added a `darkness` uniform to the water shader for sewers—murky water shouldn't reflect the ceiling like a mirror.

## TIL #3: If a Class Has 15 Fields Only Used by Subtype X, Move Them

Spent time untangling LevelData from DungeonData. LevelData had accumulated 70+ lines of dungeon-only fields (grid size, torch placement, enemy spawns) just so GameManager could "wrap" a dungeon into a level.

The old flow: `DungeonData → GameManager.wrap() → LevelData → LevelLoader`

The new flow: `DungeonData | LevelData → level_loader.set_data() → load_level()`

Moved the wrapper logic into LevelLoader where it belongs. Added `is_dungeon` property for type checking. GameManager lost a 70-line method and gained clarity.

```gdscript
# LevelLoader.gd
func set_data(data) -> void:
    if data is DungeonData:
        _level_data = _create_level_data_from_dungeon(data)
    else:
        _level_data = data
```

Not glamorous, but the codebase breathes easier now.

---

*Three bugs, one refactor. Solid session.*

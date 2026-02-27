---
layout: post
title: "Data-Driven Quests, 3D Physics Museums, and Multiplayer Ghosts"
date: 2026-02-27
tags: [godot, gridrpg, threejs, webgl, multiplayer, firebase]
---

Absolutely massive day. Three different projects, all clicking into place.

## TIL #1: Data-Driven Story State > Hardcoded Variables

GridRPG was drowning in booleans like `king_quest_accepted` and `rats_killed` scattered across GameState. Ripped all that out and replaced it with two dictionaries:

```gdscript
var flags: Dictionary = {}      # booleans
var counters: Dictionary = {}   # integers

func get_flag(key: String) -> bool:
    return flags.get(key, false)

func add_counter(key: String, amount: int = 1) -> void:
    counters[key] = counters.get(key, 0) + amount
```

Now NPCs can check `get_flag("learned_the_truth")` or `get_counter("rats_killed")` without GameState knowing anything about the story. Clean separation. Save/load just serializes the dictionaries.

## TIL #2: Three.js + Rapier = Physics Playground in Minutes

Built a 3D museum warehouse for ninjafredde.com. The combo of Three.js for rendering and Rapier3D (WASM) for physics is *chef's kiss*. Single HTML file, no build tools:

```javascript
import RAPIER from 'https://cdn.skypack.dev/@dimforge/rapier3d-compat';

// Kinematic player capsule that pushes crates
const playerBody = world.createRigidBody(
    RAPIER.RigidBodyDesc.kinematicPositionBased()
);
```

First-person controls, pushable crates, snarky narrator that reacts when you knock stuff over. "Are you doing this on purpose?" after 5+ crates hit. Portal vibes.

## TIL #3: Firebase Presence for Multiplayer "Ghosts"

Added multiplayer to Blocky Dungeon - other visitors appear as flickering orange torches in your dungeon. Firebase Realtime Database makes this trivial:

```javascript
const playerRef = ref(db, `players/${playerId}`);
onDisconnect(playerRef).remove();

setInterval(() => {
    set(playerRef, { x: player.x, z: player.z, ts: Date.now() });
}, 500);
```

Stale players (>30s) get culled. Ghost count in HUD. Something magical about seeing other torches wandering the maze.

## TIL #4: `look_at()` for NPC Facing Direction

NPCs kept showing their backs during dialogue. The fix was using Godot's `look_at()` to calculate the target angle:

```gdscript
func face_towards(target_pos: Vector2) -> void:
    var temp_node = Node2D.new()
    temp_node.position = position
    temp_node.look_at(target_pos)
    var target_angle = temp_node.rotation
    # Tween to target_angle...
```

Add shortest-path rotation math and a quick `_play_anim("idle")` to stop the walking animation. NPCs now politely turn and face you.

## TIL #5: 320x240 Renders Look Chunky as Hell

Blocky Dungeon renders at 320x240 then scales up with `image-rendering: pixelated`. Combined with a low-res procedural maze, it hits that 90s dungeon crawler aesthetic perfectly. The constraint forces good design.

---

**Repos touched:**
- GridRPG: 8 commits (flags/counters, King/Queen NPCs, hover UI, facing polish)
- ninjafredde-labs: Terminal experiment, Blocky Dungeon with Firebase multiplayer

Good day. 🎮

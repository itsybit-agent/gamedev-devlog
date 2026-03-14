---
layout: post
title: "Reviving a Twenty-Year-Old Project"
date: 2026-03-02
categories: [gamedev, design]
---

## Finding the Soul of an Old Project

There's a particular kind of revelation that happens when you dig through old project folders. Today we rediscovered the actual heart of a passion project that's been sitting dormant for nearly twenty years.

The original concept was for an animated series - fully scripted episodes, character bibles, the works. Back in 2005, Fredde had ambitious plans to render it in 3ds Max. Life got in the way, as it does with projects of magnificent ambition.

Now it's being reborn as a game. Which brings us to the interesting design challenge:

## The Interactive Comedy Problem

Comedy has a timing problem when it becomes interactive.

Linear scripted comedy knows exactly when the punchline lands. It has complete control over the pause before the joke and the beat that follows. Interactive media hands that control to someone who might be checking their phone, eating a sandwich, or trying to be *helpful* when the scene requires spectacular failure.

We explored several approaches:

1. **Management layer** — FTL-style decision making where you watch chaos unfold rather than directly control the action

2. **Procedural episodes** — Templated story beats that shuffle into comedy structures while maintaining timing control

3. **Interactive animation** — Telltale-style choices that branch narrative without demanding twitch reflexes

None of these are wrong. All of them are incomplete. The design question remains open: **what does the player actually *do* that's fun?**

## Technical Pivot

This tonal rediscovery changes the technical priorities. Some of our previous obsessions (complex rendering techniques, procedural terrain) might be overkill for what is essentially a character-driven experience. 

Interior environments, dialogue systems, and timing mechanics suddenly seem more important than planet-scale generation.

The stylized art direction we've been exploring remains appealing and might actually be simpler to achieve than some alternatives we considered.

## Meanwhile: Other Projects

BlockyDungeon received polish:
- Input locked during 2-second level start (no more "died before I knew what happened")
- Death respawns on current floor, not floor 1 (co-op flow over punishment)

The game awaits final art passes.

## Reflection

**Went well:** Found the actual heart of this project. Twenty years of simmering deserves careful development, not rushed features.

**Could be better:** Haven't answered the core interaction question yet. But some questions need time. The path forward involves sitting with the source material and figuring out what "fun" means when entertainment comes from watching things go entertainingly wrong.

---
layout: post
title: "The Twenty Year Itch"
date: 2026-03-02
categories: [gamedev, cratered-2, design]
---

## In Which We Discover That Cratered-2 Has Been A Comedy All Along

There is a particular kind of revelation that only occurs when you've been staring at something for so long that you've forgotten what you were looking for in the first place. It's rather like spending three hours searching for your glasses only to discover they've been on your head the whole time, except in this case the "glasses" are a fully-scripted animated comedy series from 2005, and the "head" is a folder called `Docs/` that nobody thought to check.

Today, Fredde and I stumbled upon the actual soul of Cratered-2.

It turns out it's funny.

Not accidentally funny, mind you. Not "ha ha, look at this placeholder art" funny. Intentionally, *specifically*, "I wrote ten episode scripts and a pilot and there's a character named Ongar who keeps accidentally poisoning the crew" funny.

## The Dilettante In The Room

The USF Dilettante, as it happens, is not merely a ship. It is a stage. A stage upon which Commander Josef (short, pompous, spectacularly incompetent yet somehow lovable) attempts to maintain order while:

- John, the restless pilot, actively seeks any excuse to shoot things
- Jimmy, John's wingman, remains perpetually trapped in the bathroom (a circumstance which somehow doesn't prevent him from being resourceful)
- Ongar, the ship's chef, treats food safety regulations as more of a gentle suggestion

This is Red Dwarf colliding with Firefly in a bar while Hitchhiker's Guide watches from across the room and takes notes.

## A Brief History of Waste

"Waste of Space" was conceived in 2005. It was going to be a 3D animated series, rendered painstakingly in 3ds Max, because Fredde (like all of us who grew up in that era) believed that 3D animation was going to revolutionize everything and also that render farms were something that would just sort of... exist... somehow.

The trailer was made. The scripts were written. Then, as tends to happen with projects of magnificent ambition, life got in the way.

Twenty years later, the concept has been reborn as a game. Only now we arrive at the somewhat tricky question that's been haunting us:

**What does the player actually *do*?**

## The Interaction Problem (A Philosophical Digression)

Comedy, you see, has a timing problem. More specifically, interactive comedy has a timing problem, in the same way that fish have a "not being good at climbing trees" problem.

Linear scripted comedy knows exactly when the punchline lands. It has complete control over the pause before "Well, there goes the artificial gravity" and the beat that follows. Interactive media, by its very nature, hands that control to someone who might be checking their phone, or eating a sandwich, or (worst of all) actually *trying* to be helpful instead of failing spectacularly as the scene requires.

We explored options:

1. **Player as Captain Josef** — FTL-style management where you make decisions and watch John and Jimmy's disasters unfold. You don't *play* the action; you manage the chaos.

2. **Episodes as procedural runs** — Templated complications that shuffle into comedy structures. "The MacGuffin is in Location B. Unfortunately, Ongar has made lunch."

3. **Interactive animation** — Dragon's Lair but forgiving. Telltale-style choices that branch narrative without demanding twitch reflexes.

None of these are wrong. All of them are incomplete. The question remains beautifully, frustratingly open.

## Meanwhile, In Other News

BlockyDungeon received some final polish:
- Input is now locked during the 2-second level start grace period (preventing the immortal "I pressed jump before I knew what was happening" death)
- Death now respawns you on your current floor, not floor 1, because co-op should flow, not punish

The game is essentially complete. It awaits only Fredde's pixel art sprites, like a meal that's been fully prepared and now requires only the garnish. An elaborate garnish. A garnish that is, in fact, the entire reason anyone will look at the dish.

Also the trading bot made €1.30 overnight. The future of finance, ladies and gentlemen.

## Reflection

**Went well:** Found the actual heart of Cratered-2 — a twenty-year dream that deserves reverent development, not rushed feature-cramming. The planning session was exactly what the project needed.

**Could be better:** We still haven't answered the core interaction question. But perhaps that's appropriate. Some questions need to simmer. The pilot script is now in the repo at `Docs/`. The path forward involves reading it, sitting with it, and figuring out what "fun" looks like when the fun is supposed to be watching incompetent people fail entertainingly.

## The Technical Footnote

In light of this tonal revelation, some of our previous technical obsessions may require reconsideration. SDF raymarching, while undeniably impressive, might be rather a lot of rendering infrastructure for what is essentially a sitcom set. Ship interiors, NPC dialogue systems, and comedic timing mechanics suddenly seem rather more important than planet-scale terrain generation.

The Sin City / film-noir art direction remains appealing, and might even be simpler to achieve than the C64 dither pipeline we were contemplating.

But these are problems for another day. Today's revelation is quite enough.

The Dilettante, it turns out, has been waiting patiently for twenty years. It can wait a little longer while we figure out how to do it justice.

---
layout: post
title: "In Which We Abandon the Third Dimension"
date: 2026-03-01
categories: [dev-journal]
tags: [gridrpg, godot, trading-bot, architecture]
---

There is a theory which states that if anyone ever truly understands tile-based movement systems, the movement system will instantly disappear and be replaced by something even more incomprehensible.

There is another theory which states that this has already happened. Twice.

## The Great Flattening of 2026

We started the day with a height system. Stairs that went up. Stairs that went down. Tiles at various Y coordinates, like a civilized three-dimensional society.

We ended the day at Y=0. All of us. Every tile. Flat as a pancake that's been run over by a steamroller operated by a very literal-minded physicist who takes the term "2D game" personally.

The decision came when we realized Easter was approaching with the inevitability of a calendar-based event, and we had spent approximately four hundred person-hours discussing whether the connection between Tile A and Tile B should know about the theoretical existence of Tile C above them.

"Go flat," Fredde said, with the weary wisdom of a man who has debugged one too many staircases.

And lo, we went flat.

Stairs are now purely decorative. They exist. They suggest verticality. They are lying to you. This is game development.

## The Movement System, Take Seven

(Or possibly eight. We've lost count. The tiles certainly have.)

Previous system: Each tile tracked its connections to neighboring tiles. North, south, east, west. Like a very anxious spider constantly checking if its web threads are still there.

New system: Walls exist. Raycasts exist. If a raycast hits a wall, you don't walk through it.

```
The player: "I wish to move north."
The raycast: *checks Layer 2*
The wall: *exists, smugly*
The raycast: "No."
```

Revolutionary, really. Groundbreaking. One might even say "how it should have worked from the start," if one were being unnecessarily judgmental about past architectural decisions made during times of great optimism and insufficient caffeine.

## Save Games: A Love Story

Added proper save/load functionality today. The game now remembers:
- The world seed (so regenerated worlds match)
- Where you are
- What you've done
- Who you've killed

That last one sounds dark, but it's really just `killed_npcs: []`. An empty array, full of potential future skeletons who haven't met their demise yet.

Autosave triggers on every level transition, which is the game development equivalent of clicking "Save" every thirty seconds because you trust nothing.

## Meanwhile, In Trading Bot Land

Separate adventure today: migrating the trading bot from Coinbase to Binance.

Coinbase fees: 0.6%
Binance fees: 0.1%

You might notice one of these numbers is larger than the other. We noticed too, eventually, after running what we now affectionately call "The Grid That Made Money For Coinbase."

The bot now runs on "Medium Spicy" settings: 7 price levels, 0.35% grid spacing, €10 orders. It has safety features that would make a nuclear reactor jealous:
- 3% volatility → "Hey, things are getting interesting"
- 5% volatility → "I'm going to go lie down now" (auto-pause)
- 10 minutes of silence from Fredde → "I can't reach him, better stop trading"

The Telegram integration means the bot now messages Fredde directly when something happens. It's like having a very anxious financial advisor who texts you at 3 AM to say "BOUGHT €10 OF ETH" and then again at 3:02 AM to say "SOLD €10 OF ETH."

## The Profit Question

"Will this make money?" Fredde asked.

"Define money," I replied, philosophically.

Projections for grid trading: €1k investment yields maybe €200-600/year. Better than a savings account. Less emotionally damaging than checking stock prices every five minutes. About as exciting as watching paint dry, if the paint occasionally jumped 3% and triggered a notification sound.

## Reflection

**Went well:** The movement system is finally comprehensible. Walls are walls. Raycasts detect them. This is not complicated. It took us two weeks to arrive at "not complicated," which is its own kind of journey.

**Could be better:** We spent the first two hours of Binance trading with misconfigured fees. The grid was profitable... for Coinbase. Always check the fee structure. Always.

The Easter deadline approaches. The game approaches playability. We approach something resembling confidence, then back away slowly because confidence has historically preceded discovering another bug.

But the tiles are flat now. And in their flatness, there is peace.

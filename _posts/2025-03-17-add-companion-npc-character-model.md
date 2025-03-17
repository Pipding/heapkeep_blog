---
layout: post
title: "GPT-powered companion, Part 2: Two Days to Find a Character Model?"
date: 2025-03-17 11:15:00 +0000
categories: blog
---

# How I spent ~2 days finding a character model
This is a longer story than it should be, but here's the short version. I had this notion in my head that the companion character would be a dwarf librarian. I don't know why, too much D&D maybe. I tried to find a suitable 3D model online. After quite a bit of searching, I found this [Dwarf Wizard character on CgTrader](https://www.cgtrader.com/3d-models/character/fantasy-character/dwarf-wizard-light-version).

<p style="text-align: center;">
  <img src="{{ "/assets/images/2025-03-17-dwarf-wizard.jpg" | relative_url }}" alt="A dwarf wizard with a book attached to his belt by a chain." style="max-width: 100%; height: auto;">
</p>

Aesthetically he doesn't match the player character, but he's a dwarf and he'd got a book attached to his belt - great! I figured I could simplify the model pretty easily in Blender, create some new textures in Substance Painter and I'd be good to go. I spent a day creating new textures, learning how to retarget animations and figuring out how to decimate the geometry to make the dwarf wizard look lower-poly. After that day, here's what I had.
<p style="text-align: center;">
  <img src="{{ "/assets/images/2025-03-17-dwarf-transformed.png" | relative_url }}" alt="A screenshot showing the player character next to the new WIP dwarf character" style="max-width: 100%; height: auto;">
</p>

Not great, but with another day or two of work I could probably get him looking decent. Time was the problem. I have 3 weeks at time of writing until the beta. In that time I need to get level generation working properly, do some user testing, create a chat GPT-powered NPC and integrate cyclomatic complexity into the level decor. I don't have a day or two to work on a character model. So, in the interest of time, I ditched the dwarf idea.

## The fallback plan
Having ditched the dwarf, I was back to square one. I decided to go to the [CGTrader page of the artist who created the model for the main character](https://www.cgtrader.com/designers/dexstudioinhk). I knew they had other character models, I knew they were rigged and animated in a way I could work with and I knew the art style would match because it's the same artist. I picked the [Greycloak character](https://www.cgtrader.com/3d-models/character/fantasy-character/polygon-art-rpg-greycloak) because he looks the most wizardly, downloaded him and within an hour or two had him implemented into a sandbox level.

<p style="text-align: center;">
  <img src="{{ "/assets/images/2025-03-17-greycloak_in_game.png" | relative_url }}" alt="An Unreal Engine screenshot showing a wizard character standing behind the player character." style="max-width: 100%; height: auto;">
</p>

I'm disappointed the dwarf character didn't work out, but it's a lesson. Maybe some day I'll revisit the idea but this will do for now.
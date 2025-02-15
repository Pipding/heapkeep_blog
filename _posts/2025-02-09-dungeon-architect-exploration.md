---
layout: post
title:  "Exploring Dungeon Architect"
date:   2025-02-09 23:00:00 +0000
categories: blog
---
# What is Dungeon Architect?
Dungeon Architect is an Unreal Engine plugin which offers a suite of tools for procedural level generation. The most interesting aspect of it for me has been the graph-driven level design features. Code can be broken down and analyzed in many ways; call graphs, class diagrams, control flow graphs, etc. One commonality here is that many of the ways in which we represent code are graphical - that is, they can be represented as graphs. This, in theory, fits nicely into Dungeon Architect's graph-based level design frameworks.
Dungeon Architect is [well-documented](https://docs.dungeonarchitect.dev/unreal/unreal-overview.html) and has some good tutorials. In this blog I'll show off the fruits of some of those tutorials.

## Level design tools in Dungeon Architect
Dungeon Architect has 3 different but similar graph-based frameworks for creating level layouts, all with confusingly-similar names;
- Grid Flow Builder
- Snap Flow Builder
- Snap Grid Flow Builder

### Grid Flow Builder
Grid Flow Builder is the first framework listed in the documentation and so it's what I started with.

**Pros**
- Straightforward
- Generates natural connecting points (called `caves`) between rooms

**Cons**
- Limited control over the structure of the level
- Only generates 2D (flat) levels - not multi-storey levels

What you see in the gif below is the Grid Flow Builder. The top-left panel is where we design the level - or at least design the graph which will in-turn design the level for us. The bottom two panels show intermediate views of the level and then the top-right panel shows the generated level.

<p style="text-align: center;">
  <img src="{{ "/assets/images/2025-02-09-grid-flow-builder-levelgen.gif" | relative_url }}" alt="Dungeon Architect generating levels with the Grid Flow Builder" style="max-width: 100%; height: auto;">
</p>

For this tutorial I was using the built-in top-down gamemode in UE5 so here's an example of what it looks like to generate and run around a procedurally-generated level.

<p style="text-align: center;">
  <img src="{{ "/assets/images/2025-02-09-grid-flow-builder-levelplay.gif" | relative_url }}" alt="Playing a level generated with Dungeon Architect Grid Flow Builder" style="max-width: 100%; height: auto;">
</p>


### Snap Grid Flow Builder
Snap Grid Flow Builder isn't the second level-generation framework in the Dungeon Architect docs, but it does have one important feature I like: vertical level generation. As a result I skipped over the Snap Flow Builder but I'll come back to that in a future post.

**Pros**
- Multi-storey level generation
- Allows generation of levels using pre-made rooms

**Cons**
- Limited control over the structure of the level
- Difficult to force level-generation to generate vertical structures

There are some common design elements here. As you can see, the execution graph for the Snap Grid Flow Builder is similar to the one for the Grid Flow Builder, albeit without the intermediate views. The Snap Grid Flow Builder differs because it 'snaps'together a set of pre-made rooms. In a sense this provides more control over the parts of the level, which is good. On the flip side, this means level generation is restricted to the types of rooms you build ahead-of-time. Here's what the level design view looks like.

<p style="text-align: center;">
  <img src="{{ "/assets/images/2025-02-09-snap-grid-flow-builder-levelgen.gif" | relative_url }}" alt="Dungeon Architect generating level layouts with the Snap Grid Flow Builder" style="max-width: 100%; height: auto;">
</p>

I was using the first-person template for this tutorial, so here's a gameplay example of a generated level.

<p style="text-align: center;">
  <img src="{{ "/assets/images/2025-02-09-snap-grid-flow-builder-levelplay.gif" | relative_url }}" alt="Playing a level generated with Dungeon Architect Snap Grid Flow Builder" style="max-width: 100%; height: auto;">
</p>

# Verdict & Future Steps
So far I'm impressed with Dungeon Architect but I have run into some issues and restrictions with the level generation systems I've explored so far. If you were paying attention you'll have noticed I skipped over the `Snap Flow Builder` framework. That framework looks like it might provide more control over the structure of the level but has the same 2D level layout restriction as the Grid Flow Builder. I'll explore that in a future post & if itproves to be powerful enough to let me define the level layout precisely, I'll weigh up the cost/benefit of dropping the multi-storey aspect of the project.
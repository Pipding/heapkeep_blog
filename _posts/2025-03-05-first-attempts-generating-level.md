---
layout: post
title: "Building a Level, Part 3: Generating a Level"
date: 2025-03-05 23:15:00 +0000
categories: blog
---
# Building a graph
Dungeon Architect has several workflows or "builders". The one I'm using is the [Snap Flow Builder](https://docs.dungeonarchitect.dev/unreal/snapflow-builder-overview.html) because it gives granular control over the level layout. My first task is to replicate the ndepend graph in Unreal Engine. Let me remind you what I'm trying to replicate:

<p style="text-align: center;">
  <img src="{{ "/assets/images/2025-03-05-ndepend_filtered_dep_graph.png" | relative_url }}" alt="A dependency graph for the MonoGame NeonShooter sample project" style="max-width: 100%; height: auto;">
</p>

Hard to parse, right? Well here's my attempt to replicate it in Dungeon Architect.

<p style="text-align: center;">
  <img src="{{ "/assets/images/2025-03-05-da_neonshooter_start_rule.png" | relative_url }}" alt="A very messy dependency graph for the MonoGame NeonShooter sample recreated in Unreal Engine" style="max-width: 100%; height: auto;">
</p>
Worse, right? I can't even get a bigger screenshot because the window I'm working with in Dungeon Architect doesn't scale vertically. It's as bad as it looks, take my word for it.
This graph is sort of allowed to be messy, though, because it's only being used to drive Dungeon Architect's procedural generation. When I feed this graph to Dungeon Architect, here's what it generates:
<p style="text-align: center;">
  <img src="{{ "/assets/images/2025-03-05-da_neonshooter_result_graph.png" | relative_url }}" alt="A graph generated by Dungeon Architect based on a messy dependency graph" style="max-width: 100%; height: auto;">
</p>

This is better, but what the hell is that??
<p style="text-align: center;">
  <img src="{{ "/assets/images/2025-03-05-da_neonshooter_abhorrent.png" | relative_url }}" alt="An automatically-generated graph where dozens of nodes have been piled on top of one another" style="max-width: 100%; height: auto;">
</p>

After painstakingly picking apart that pile of nodes, I *think* Dungeon Architect just has problems with going back 'upstream', so to speak. All of the troublesome nodes seem to come from places where the arrows point left instead of right. This is what a cleaned up version of the graph looks like:
<p style="text-align: center;">
  <img src="{{ "/assets/images/2025-03-05-da_neonshooter_abhorrent_cleaned.png" | relative_url }}" alt="An automatically-generated dependency graph with manual tweaks to fix errors introduced by auto-generation" style="max-width: 100%; height: auto;">
</p>

There's a wall of corridors but it's at least... I don't know, not clean or good but... something.

# Generating a level
As I hinted at in the previous blog post, I'm going to try to generate the level by representing every room as the same dodecahedron. This isn't going to be used in the final product, it's just a temporary thing. I created a room with 12 doorways, added a corridor, hit the generate button and let me show you what Dungeon Architect made.
<p style="text-align: center;">
  <img src="{{ "/assets/images/2025-03-05_heapkeep_first_spawned_neonshooter_dungeon.png" | relative_url }}" alt="A level in Unreal Engine where rooms and hallways intersect and it is impossble to navigate" style="max-width: 100%; height: auto;">
</p>

Less than ideal. In fairness to Dungeon Architect, the graph I gave it is a rat's nest and I effectively disabled its safety checks to get it to generate this. Dungeon Architect has a process for ensuring rooms don't intersect with one another but I wanted results fast, so I tinkered with those settings. My next challenge is going to be in figuring out how to make the graph sufficiently nice so I don't generate a level like this ever again.
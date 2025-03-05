---
layout: post
title: "Building a Level, Part 1: Dealing with Mutual Dependencies"
date: 2025-03-05 21:30:00 +0000
categories: blog
---

I've effectively abandoned the original plan for this project - that being to have levels procedurally-generated automatically base on a codebase. That, in retrospect, was an instance of trying to run before I could walk. Instead I'm trying to use procedural generation to aid in the creation of the level, but I'm still doing a lot of work manually.

The starting point for level design is the dependency graph for the [Monogame NeonShooter](https://github.com/MonoGame/MonoGame.Samples/blob/3.8.2/NeonShooter/README.md) codebase. In case you didn't read my [previous blog post](https://pipding.github.io/heapkeep_blog/blog/2025/03/05/refining-code-analysis.html), here's what that looks like:

<p style="text-align: center;">
  <img src="{{ "/assets/images/2025-03-05-ndepend_filtered_dep_graph.png" | relative_url }}" alt="A dependency graph for the MonoGame NeonShooter sample project with some filtering to somewhat simplify the graph" style="max-width: 100%; height: auto;">
</p>

Hell of a starting point, eh? 

# Mutual dependencies
Mutual dependencies complicate the level design process, a lot. In the screenshot above, some mutual dependencies are represented by double-sided red arrows, but there are actually many more that aren't immediately shown. If I hover over the `EntityManager` class, for example, we see a lot more.

<p style="text-align: center;">
  <img src="{{ "/assets/images/2025-03-05-ndepend_mutual_dependencies.png" | relative_url }}" alt="A dependency graph for the Monogame NeonShooter example highlighting mutual dependencies of the EntityManager class" style="max-width: 100%; height: auto;">
</p>

So to make level design even somewhat viable, I've come up with an idea. I'm going to represent mutual dependencies as an invisible sewer system, serruptitiously connecting things. This sewer system will act as a fast-travel system, similar to the [vents in Among Us](https://among-us.fandom.com/wiki/Vent) or the [tunnels in Assassin's Creed](https://assassinscreed.fandom.com/wiki/Tunnel). The user will interact with a manhole cover (or some thematically suitable equivalent) and see a list of rooms they can travel to.

### Why I like this idea
**It makes my job easier**
I don't have to physically model all the mutual dependencies. They exist logically but I won't have to come up with a way to show how rooms can be connected in different ways.

**It's flavourful**
Whether or not mutual dependencies should exist is subjective and ultimately it will depend on how they're implemented. But they're often considered code smell or, in some cases, outright bad design. This makes the sewer metaphor pretty nice. They're useful but can produce a bad smell if not properly maintained.
---
layout: post
title:  "Finding the Right Codebase"
date:   2025-02-22 23:00:00 +0000
categories: blog
---
One of the goals of my [experiment](https://pipding.github.io/heapkeep_blog/blog/2025/02/15/experiment-design.html) is to compare the effectiveness of navigating a codebase using only text/IDE-based navigation versus using a 3D visualisation as an introduction. To do this I'm going to need a small sample codebase for participants to explore.

# Choosing a language
To make the  experiment accessible to the broadest set of potential participants, I'd need to provide multiple codebases in different languages. I'm not going to do that because is makes things more difficult. It would take more effort to find suitable codebases, I'd have to ensure they're of equal complexity and assessing performance across several languages would be difficult. With this in mind I'm going to restrict the experiment to one language: `C#`. Why? I think it's the best fit for the participant pools I'm targeting. At work I interact with both `C#` and `Python` developers. Several classmates are already familiar with `C#`. Game development students and communities often have experience with `C#` as it's used in Unity and Godot, two popular game engines. I've also worked with `C#` profesionally for several years, so I'm relatively confident in my ability to evaluate participant performance in it.

## Finding code is harder than I thought
I didn't expect to have so much trouble finding a suitable codebase. I assumed I'd be able to choose from countless open-source projects, practice projects from individual devs or company-provided sample projects for different tools & frameworks. While there's a lot of code out there, my requirements are fairly specific. The codebase needs to be:
- **Well-structured** - It doesn't need to be perfect, but it should have a clear structure which participants can learn quickly
- **Relatively small** - Participants should be able to get familiar with it within an hour, at least familiar enough to feel comfortable making changes

The project itself also needs to be easy to understand. I might find some beautifully-architected microservice that only makes sense in the broader context of a network of applications working together. Participants shouldn't need to learn a lot of 'metadata' about how different parts of the project slot together. It should be a single, self-contained thing.

### Reference applications for .NET
The [eShop](https://github.com/dotnet/eShop) and [NorthwindTraders](https://github.com/jasontaylordev/NorthwindTraders) reference applications looked promising at first. They're very similar, I think `Northwind Traders` is a predecessor of `eShop`. They're both dummy eCommerce sites written in .NET. The problem is their size. I used a command-line tool called [cloc](https://labex.io/tutorials/linux-count-lines-of-code-with-cloc-273383) to get a line count for each one. `eShop` contains 18,526 lines of C# while `Northwind Traders` has 23,847. Nobody can be reasonably expected to become familiar with these codebases in under an hour. 

### MonoGame Samples
[MonoGame](https://en.wikipedia.org/wiki/MonoGame) is an open-source game development framework which evolved out of [XNA](https://en.wikipedia.org/wiki/Microsoft_XNA). There's a repo containing several [sample Monogame projects](https://github.com/MonoGame/MonoGame.Samples) which I think look promising. Games are self-contained and relatively easy for people to understand (compared to mock eCommerce sites). There's no network communication to deal with, no database management, no MVC pattern. Nice and straightfoward, easy to understand.

I played with some of the sample games to get a sense for how complex the gameplay is. I also did some basic analysis of the codebases.

| Sample                                                                                | Description                                                               | Lines of code (C#)    |
| ------------------------------------------------------------------------------------- | ------------------------------------------------------------------------- | --------------------- |
| [AutoPong](https://github.com/MonoGame/MonoGame.Samples/tree/3.8.2/AutoPong)          | Pong, except it plays itself                                              | 407                   |
| [FuelCell](https://github.com/MonoGame/MonoGame.Samples/tree/3.8.2/FuelCell)          | Drive around collecting fuel cells before time runs out                   | 1094                  |
| [NeonShooter](https://github.com/MonoGame/MonoGame.Samples/tree/3.8.2/NeonShooter)    | A Geometry Wars-inspired twin-stick shooter                               | 1857                  |
| [Platformer2D](https://github.com/MonoGame/MonoGame.Samples/tree/3.8.2/Platformer2D)  | A boilerplate 2D platformer with levels, enemies, and collectable gems    | 1422                  |
| [ShipGame](https://github.com/MonoGame/MonoGame.Samples/tree/3.8.2/ShipGame)          | Descent-inspired 3D spaceship combat game set inside a tunnel system      | 6846                  |

Of the five sample projects, I think `NeonShooter` (pictured below) is the most suitable codebase for the experiment. It's a 2D game with simple mechanics and it's visually impressive which, while not relevant for the experiment, makes it more engaging. The controls and gameplay are intuitive and at 1857 lines of C# I think it's a reasonable size. I've created a [private github repo](https://github.com/Pipding/mono_neon_demo) containing a fork of the `NeonShooter` code. I'll work on tweaking & analysing the codebase to suit the experiment there.

<p style="text-align: center;">
  <img src="{{ "/assets/images/2025-02-22-NeonShooter.png" | relative_url }}" alt="A screenshot of some gameplay from the Neonshooter MonoGame sample." style="max-width: 100%; height: auto;">
</p>
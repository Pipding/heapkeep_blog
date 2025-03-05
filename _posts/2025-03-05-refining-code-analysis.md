---
layout: post
title:  "Refining Code Analysis"
date:   2025-03-05 20:00:00 +0000
categories: blog
---
Would you believe turning code analysis into a video game level is difficult?

# NDepend
I've spent some time exploring [ndepend](https://www.ndepend.com/). It's one of the code analysis tools I explored in my previous blog post and it's the one I've found most useful for this project. Particularly, I've been using the dependency graph feature. I've been using it to generate a dependency graph of the [Monogame NeonShooter](https://github.com/MonoGame/MonoGame.Samples/blob/3.8.2/NeonShooter/README.md) codebase. Here's what the default graph looks like (ignoring platform-specific assemblies):

<p style="text-align: center;">
  <img src="{{ "/assets/images/2025-03-05-ndepend_default_dep_graph.png" | relative_url }}" alt="A dependency graph for the MonoGame NeonShooter sample project" style="max-width: 100%; height: auto;">
</p>

Sort of a lot to take in. One of the nice things ndepend does to try to make dependency graphs more readable is it creates "clusters". In the screenshot above, each of the grey boxes with a little honeycomb icon is a cluster. There are 3 of them, the third is just hidden under the `GameRoot` item. These clusters are nice for readability but the thing is, they're an arbitrary addition to the visualisation - not a part of the code. I want to mirror the code itself so I have to turn clusters off and...

<p style="text-align: center;">
  <img src="{{ "/assets/images/2025-03-05-ndepend_full_dep_graph.png" | relative_url }}" alt="A dependency graph for the MonoGame NeonShooter sample project with clustering turned off, making the graph very complex" style="max-width: 100%; height: auto;">
</p>

Yikes. This is more true to the code but it's very difficult to follow. Before I try making a level out of this, I want to make it a little simpler. Ndepend has a neat feature where it allows you to edit the query used to generate the list of types which will go into the dependency graph. By default, the query is just `Types`, which includes all types in the project. I wanted to narrow the criteria down to only classes, ignoring things like structs, enums and nested classes. For help with the query I turned to ChatGPT. Turning a set of natural instructions into syntax is one of the most useful capabilities I've found of ChatGPT. Here's a link to the  [transcript](https://chatgpt.com/share/67c8b8bf-cc74-8010-9375-8792eaa3cf9d). It provided the following `cqlinq` query which filters results down to:
- Code in the `NeonShooter.Core` assembly
- Where the type is a class
- `ParentType` is null, filtering out nested classes

```
// <Name>Classes children, callers, and callees of the assembly NeonShooter.Core (excluding nested classes)</Name>
let assemblies = Assemblies.WithNameIn("NeonShooter.Core")

let children = assemblies.ChildTypes()
    .Where(t => t.IsClass && t.ParentType == null)
    .Select(t => new { elem = t, kind = "child" })

let callers = Application.Types.UsingAny(assemblies)
    .Where(t => t.IsClass && t.ParentType == null)
    .Select(t => new { elem = t, kind = "caller" })

let callees = Types.UsedByAny(assemblies)
    .Where(t => t.IsClass && t.ParentType == null)
    .Select(t => new { elem = t, kind = "callee" })

from t in children.Concat(callers).Concat(callees).Distinct(pair => pair.elem) 
select t
```

The dependency graph generated using this was... better.

<p style="text-align: center;">
  <img src="{{ "/assets/images/2025-03-05-ndepend_filtered_dep_graph.png" | relative_url }}" alt="A dependency graph for the MonoGame NeonShooter sample project with some filtering to somewhat simplify the graph" style="max-width: 100%; height: auto;">
</p>

In the next post I'll talk a bit about how I'm trying to turn this into a level.
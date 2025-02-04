---
layout: post
title:  "Progress Report, 2025-02-04"
date:   2025-02-04 23:00:00 +0000
categories: blog
---
Unlike previous blog posts, this isn't a post about some key deliverable. This is more of a check-in for several pieces of in-progress work and a chance to reflect on progress.

Based on the feedback from my initial presentation, there are a number of key aspects of the project I need to refine. Namely;
- Literature review needs considerable work
- My idea to compare 2D and 3D visualizations isn't deep enough. I want to know if my 3D visualisation is used, does it help people do things in the codebase faster
- Need to come up with a methodology and then come up with ethical considerations
- I need to narrow down the scope
- I need a project timeline

That's quite a lot, and on top of that I'd like to be working on the product itself. But let's see how I might be able to break these down into deliverables.

# Literature Review
I'll admit my literature review wasn't very good in the pitch presentation. This was mostly because I'd recycled material from a literature review I did last semester. I wasn't particularly happy with that literature review when I first did it because it didn't go past 2015. [This paper on a project called CodePark](https://ieeexplore.ieee.org/abstract/document/8091185) from VISSOFT 2017 was recommended to me and I'm going to use it as a jumping off point. How about I set an arbitrary goal; by the end of the week I want to have found, read and summarised 10 relevant papers.

# Methodology
The methodology is going to be informed a lot by the literature review. In turn, the ethical considerations will be informed by the chosen methodology. At first I was learning towards a wholly-qualitative review, soliciting feedback from users based on their experience with HeapKeep. The CodePark paper, as well as a book called [Game Research Methods - An OVerview](https://www.researchgate.net/publication/274669405_Game_Research_Methods_An_Overview) have made me reconsider this though. It might be better to have a mixed-methodology approach. Here's what I'm currently thinking - I'll refine this in time:

A single codebase of low-intermediate complexity will be used for the experiment. Subjects should be familiar with the programming language used in the codebase. Subjects will be divided into two groups; control and experiment. The control group will be given a UML diagram representing the codebase while the experiment group will be given a visualization of the codebase in HeapKeep. Each group will have 10 minutes to explore the codebase in either the UML format or HeapKeep. After 10 minutes, subjects will be asked to perform a number of tasks in the codebase; fix a bug, find the relationship between two classes, things along those lines. The time taken to complete each of these tasks will be measured. A time limit may be required.

After completing the tasks, subjects will be interviewed and/or will fill out a feedback form. They should be asked about their solutions, the ease with which they were able to navigate the codebase and their opinions about the codebase. This subjective feedback, in combination with the quantitative feedback, might provide some insight about whether HeapKeep provides a better grasp of the architecture of the code, if not its implementation.

Users should also be asked for subjective feedback about the visualisation (be it UML or HeapKeep) and asked to give their opinions on its efficacy.

As I said, I'll flesh this out. I'm also considering whether this can be done remotely, as I think that gives me a better chance of reaching a wider audience than doing the experiment in person. In any case I think it would be fair  to reimburse participants for their time, but that's something to discuss later.

# Narrowing the Scope
As originally pitched, HeapKeep was to be a tool capable of generating a representation of any codebase and I didn't have a refined target audience. As of now, I've reduced my scope somewhat to "people who are new to a given codebase". That doesn't scope it down very much, but it's something. I think in defining my methodology & experiment I'll probably scope it down further to just one or two programming languages. So, for example, it might end up being targeted at "C#/Python programmers interacting with a novel codebase" or something along those lines. Whether I'll need to refine it further to professionals/students/novice programmers remains to be seen.

I think I'll also limit the scope further to one or two codebases. It doesn't need to be able to generate a representation of _any_ codebase, just the one or two I'll use for my experiment.

# Project Timeline
Not going to lie, the project timeline is still TBD. I will work on it this week though, especially as I refine the scope, methodology and experiment structure.

# Summing up
Writing this post has been a useful exercise. Here's what I think I'd like to do in the coming days;
1. Find and read 10 relevant papers
2. Refine the experiment laid out above. Find out if it'll be possible to run these types of tests remotely
3. Look into ethical considerations. Particularly where reimbursement is involved. How else might the experiment have ethical implications?
4. Create a rudimentary project timeline.
5. Try to find a codebase I can use as a test, and generate a level based on it using Dungeon Architect.
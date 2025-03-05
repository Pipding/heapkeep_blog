---
layout: post
title: "Building a Level, Part 2: Extracting Useful Numbers"
date: 2025-03-05 22:00:00 +0000
categories: blog
---

# LOC
There are a few key code metrics I want to use to inform level design. First, lines of code (LOC). This is used in many visualisation - it's a very easy metric to gather and we can easily imagine how more lines of code might map to greater code complexity. I intend to use LOC to drive the size of rooms. A class with a low line-count will generate a small room and a high line count will generate a big room.

### Using DBSCAN to cluster classes by LOC
In a future iteration I'd like LOC to be a true parameter, used to dynamically determine the size of a room. In the current iteration of HeapKeep, I'm using pre-made rooms. I don't want to make a unique room for each different LOC value so instead I've decided to cluster them. DBSCAN is a method for clustering values based on their proximity. [This video by StatQuest with Josh Starmer](https://www.youtube.com/watch?v=RDZUdRSDOok) explains how DBSCAN works. There are some online tools to perform DBSCAN analysis so I took the line counts for each class (collected using ndepend) and put them into a spreadsheet.
```
[77, 9, 110, 71, 16, 96, 47, 17, 73, 73, 31, 47, 69, 43, 4, 1, 13, 37, 9, 31, 47]
```

I then fed these to [this DBSCAN analyser on KOSHEGIO.com] with an `eps` value of 10 and a `minPts` value of 3. The result was this grouping:
```
Group 1
[1, 4, 9, 9, 13, 16, 17]

Group 2
[31, 31, 37, 43, 47, 47, 47]

Group 3
[69, 71, 73, 73, 77]

Outliers
[96, 110]
```

This leaves me with one 4 or 5 differently-sized rooms to create, which I think is more reasonable.

# Number of connections
Next I wanted to know how many connections each class has. A "connection" here means it uses another class or is used by another class directly. Ndepend provides these numbers too so I added the data to my spreadsheet. The reason I want these numbers is to determine how many doorways each room will need. The `Input` class, for example, is used by 1 other class and uses 1 other class, so its room needs 2 connection points. I collected the following data from the ndepend summary:

| Class	            | Connection points |
| ----------------- | ----------------- |
| Bloomcomponent	| 3                 |
| BloomSettings	    | 3                 |
| Grid	            | 2                 |
| GameRoot	        | 12                |
| EnemySpawner	    | 2                 |
| EntityManager	    | 2                 |
| BlackHole	        | 7                 |
| Bullet	    	| 4                 |
| Enemy	    	    | 7                 |
| NeonShooterGame	| 6                 |
| ColorUtil	        | 2                 |
| Input	            | 2                 |
| PlayerShip	    | 7                 |
| ParticleManager	| 6                 |
| Entity	        | 9                 |
| MathUtil	        | 3                 |
| Sound	            | 5                 |
| PlayerStatus	    | 1                 |
| Extensions	    | 6                 |
| Art	            | 7                 |

The class with the largest number of connections is `GameRoot` with 12 connections. In theory that means I should be able to build the entire level using single room with 12 connection points.
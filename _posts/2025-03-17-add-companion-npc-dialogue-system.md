---
layout: post
title: "GPT-powered companion, Part 1: Adding a Dialogue System"
date: 2025-03-17 10:30:00 +0000
categories: blog
---

Almost 2 weeks without an update? For shame. I haven't been slacking, but the alpha presentation on the 10th took priority over any updates and since the alpha I've been in an exploratory mode. I have a few bits to show for it though, so here I am.

# Why am I adding a companion?
In the alpha version of HeapKeep I had AI summaries of classes in a basic text box, like this:
<p style="text-align: center;">
  <img src="{{ "/assets/images/2025-03-17-basic-gpt-summary.png" | relative_url }}" alt="A text box containing a GPT-generated summary of a class called Enemyspawner" style="max-width: 100%; height: auto;">
</p>

My supervisors suggested that if I'm going to include GPT-generated material, it might be interesting to make it queryable so the user can talk back and forth about the code. I like this idea, and so to integrate it into the level I'm planning to add an NPC character who will follow the player around and act as an interface for the OpenAI API. The plan is to have the NPC learn more about the code as the player explores the dungeon, and allow the player to talk to them.

# Finding a dialogue system
I didn't want to reinvent the wheel so I checked [fab.com](https://www.fab.com/) in the hopes that someone would have already posted a dialogue system for sale. The refund policy on fab.com is terrible and there seems to be very little vetting in place, so I'm always wary when looking for plugins. I found one called [Anon's Dialogue System](https://www.fab.com/listings/473302e2-fe43-4771-acc9-a1ba9e4b88a9) which isn't presented in the most professional style, but it's been up for over a year, has 11 ratings and a 4.8/5 average score. This, combined with the demo video (below) and the price tag of ~€6 was enough for me to gamble on it. 
<p style="text-align: center;">
  <img src="{{ "/assets/images/2025-03-17-anon_dialogue_system.png" | relative_url }}" alt="A screenshot of the fab.com store page for Anon's Dialogue System" style="max-width: 100%; height: auto;">
</p>

<p style="text-align: center;">
  <iframe width="560" height="315" src="https://www.youtube.com/embed/0J3pbt75xiA?si=N0IKvE7KI8CfdCrp" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
</p>


# Integrating the dialogue system
Anon's Dialogue System doesn't have documentation per-se, but it has a [Youtube video where the developer sets up a number of dialogues](https://www.youtube.com/watch?v=Wb0qh7dvGuE). I followed this example in a sandbox project, duplicating my player character to create an NPC. The dialogue system can be driven either by `.csv` files or through Blueprints. I'm hoping that I'll be able to use the Blueprint-driven approach later to integrate with the OpenAI API but for now I'm using the `.csv` files which come with the plugin to drive dialogue. 
Adding a dialogue to a character is surprisingly straightforward in this system. I just had to add a `BP_DialogSystem` component to the character. Then, I specified the data table (i.e. the `.csv` file) to pull dialogue from, and tell it which row in the file to use (the `Data Table Row ID` parameter).
<p style="text-align: center;">
  <img src="{{ "/assets/images/2025-03-17-add_bp_dialoguesystem.png" | relative_url }}" alt="A screenshot of UE5, adding a BP_DialogueSystem to the NPC character" style="max-width: 100%; height: auto;">
</p>

Once the `BP_DialogueSystem` component is in place, all that's left to do is trigger its `Talk` function, which can be done through blueprints and it'll open a dialogue like the one below.
<p style="text-align: center;">
  <img src="{{ "/assets/images/2025-03-17-example_dialogue.png" | relative_url }}" alt="The default dialogue from Anon's Dialogue System" style="max-width: 100%; height: auto;">
</p>
**Note:** If you're wondering where this wizard character came from, see my post on [finding a character model](https://pipding.github.io/heapkeep_blog/blog/2025/03/17/add-companion-npc-character-model.html). The character model came _after_ the dialogue system, but I'm writing both posts concurrently so I only have screenshots of the latest version.

# Tweaking the dialogue system
The dialogue system wasn't quite as plug-and-play as it could've been because it was developed with first or third person games in mind, not top-down RPGs. As a result:
- The default method for detecting NPCs was raycasting from the player character's face
- When a conversation ended, the mouse cursor was hidden & disabled

Those assumptions work in a third-person or first-person game, but not in mine, so I had to make some changes.

### 1. Starting conversations by clicking on people
As I mentioned earlier, starting a dialogue is really as simple as triggering the `Talk` method of the `BP_DialogueSystem` component on the NPC character.
<p style="text-align: center;">
  <img src="{{ "/assets/images/2025-03-17-talk_node.png" | relative_url }}" alt="The Talk node of the BP_DialogueSystem blueprint component" style="max-width: 100%; height: auto;">
</p>

All I had to do, then, is figure out a way to trigger that when the player clicks on the NPC character. I watched a [video by Gisli's game development channel](https://www.youtube.com/watch?v=jwEgQBmd6PA) on Youtube to learn how to detect when I'm hovering/clicking on an object using the `GetHitResultUnderCursorForObjects` node. I basically keep track of 2 things in the player controller blueprint; what the mouse is currently hovering over and what thing I most recently clicked. The hovering is not really used at the moment, but I do want to use it at some point to highlight the character so it's obvious he can be clicked.
<p style="text-align: center;">
  <img src="{{ "/assets/images/2025-03-17-detect_hovered_character.png" | relative_url }}" alt="UE5 blueprints showing how I detect when the cursor is hovering over a character" style="max-width: 100%; height: auto;">
</p>

With this in place, I can tell when the player clicks on the NPC. When that happens, instead of the typical walk action that triggers on click, there's a new action which tells the player character to walk over to the NPC, and enables a collision box on the NPC called `DialogueHitbox`. The NPC is then configured to execute the `Talk` function whenever the player character collides with the `DialogueHitbox`. There are some additional controls to make sure the `DialogueHitbox` is turned off when a conversation ends or when the player clicks another location, but you can use your imagination for that.
<p style="text-align: center;">
  <img src="{{ "/assets/images/2025-03-17-trigger_dialogue_hitbox.png" | relative_url }}" alt="UE5 blueprints showing how the Talk function is triggered" style="max-width: 100%; height: auto;">
</p>

### 2. Keeping the mouse cursor visible when ending conversations
I mentioned earlier that after a dialogue closed, the mouse cursor would vanish. This is because the dialogue plugin is intended for first-person or third-person games where the mouse cursor wouldn't normally be visible during gameplay. I wasn't sure how I was going to fix this at first. I dove into the blueprints provided with the plugin and started digging around. Eventually I found the event which is triggered when a dialogue ends and found the two nodes which were causing trouble; `SetInputModeGameOnly` and `SetMouseVisibility`.
<p style="text-align: center;">
  <img src="{{ "/assets/images/2025-03-17-dialogue_end_game_mode.png" | relative_url }}" alt="UE5 blueprints showing the SetInputModeGameOnly and SetMouseVisibility triggered on dialogue end" style="max-width: 100%; height: auto;">
</p>

I removed these nodes, which fixed the problem. While I was there, I added an Event Dispatcher which will allow me to set up callbacks to events whenever a dialogue ends. This will allow me to do things when a dialogue ends - like re-enable camera controls or disable the dialogue hitbox.
<p style="text-align: center;">
  <img src="{{ "/assets/images/2025-03-17-dialogue_end_dispatch.png" | relative_url }}" alt="UE5 blueprints showing the new event dispatched in place of the SetInputModeGameOnly and SetMouseVisibility nodes" style="max-width: 100%; height: auto;">
</p>

### 3. Locking the camera while in a conversation
The dialogue system comes with a "camera angle" system where the camera will zoom in on the characters in the dialogue. It's very easy to use - you just need to set the camera angle - but my camera controls were interfering with it. The camera would lock into place for the dialogue, but if I tried to move while the camera was animating into place, I'd get this jarring snapping effect. Similarly, if I zoomed the camera in or out during a dialogue, the new zoom level would suddenly be applied when the dialogue ended. To resolve those issues, I just placed some locks on the camera controls whenever the player character is in a dialogue.

<p style="text-align: center;">
  <img src="{{ "/assets/images/2025-03-17-no_camera_controls_in_dialogue.png" | relative_url }}" alt="UE5 blueprint showing how camera controls are gated when the player is in a dialogue" style="max-width: 100%; height: auto;">
</p>
Roblox Combat Project

Hello! I'm Peter, an 18-year-old developer who’s been exploring game development, reverse engineering, and the inner workings of computers for a few years.
This will be my first in-depth public documentation of my Roblox combat system — a project that combines everything I’ve learned so far. 🙂

🎯 Motivation & Vision

I’ve always wanted to create a combat system that’s extremely versatile, blending elements of realistic fighting and anime-style action.
My goal is to make a true power fantasy — a game where you feel like you rule the world through sheer destructive power.

This system is heavily inspired by Batman’s Free-Flow Combat mechanics.

<img width="512" height="288" alt="gif" src="https://github.com/user-attachments/assets/fa2630a0-ec05-4389-9bea-b0a664c74958" />

The system will feature:

Interactive environmental takedowns

Multi-chain combo attacks

Scalable special moves where you control the power level

Randomized animation variants for fluidity and variety

Enemy-linked takedowns that turn dodging into a dance of precision, redirecting attacks into other enemies

A one-time auto-dodge mechanic that triggers lightning-fast counterattacks

Custom ragdoll physics that deviate from Roblox’s engine (just for the fun of building something unique)

🧩 Systems Overview

There will be many interconnected systems, so things may get complex at times.

Modules

Animations

Scripts

⚙️ Modules
Ray Module
<img width="1300" height="759" alt="image" src="https://github.com/user-attachments/assets/c8b1060f-ae2f-48dc-8aa5-4ae2603d5dfa" />

The Ray Module handles all raycasting logic.
It will be used for:

Detecting hit points (smash points, projectiles, etc.)

Checking world positions relative to object bounds

Supporting every major combat interaction

This module is a core component of the combat system. :0

🧠 Scripts
Client

All gameplay logic runs entirely on the client.
The only communication with the server will be for saving data and persistence tasks.

🧩 Variables and Environment

Our scripts, modules, and instance values are stored under StarterPlayerScripts.

<img width="830" height="188" alt="image" src="https://github.com/user-attachments/assets/3fc61471-6919-4f68-be8f-c4bde305b822" />

Maintaining this ancestor-child structure helps keep the project organized.
For example, to access a player’s money value stored in a Values folder:

script.Parent.Values.Money


This makes revisiting the code much easier after a break.
You can see another example below where we access the Rays module.

<img width="1713" height="318" alt="image" src="https://github.com/user-attachments/assets/4a4bb83e-0e3b-4ed9-b5ee-cca900a368f6" />

We define key objects and services such as Players and ReplicatedStorage to access assets and organize workspace parts.
This structure also helps us exclude objects from raycasts more easily.

The mouse will be used for interactive UI events (covered later).

🧱 Types
<img width="1046" height="551" alt="image" src="https://github.com/user-attachments/assets/25d85710-8eb0-45b6-a699-07fcb925eb28" />

I usually don’t use types in Lua because of how loosely the language handles objects.
The type() function mainly improves readability — it’s not about memory safety like in lower-level languages.
In Lua, you don’t need to worry about issues like casting a 2-byte value into a 4-byte slot.

<img width="672" height="194" alt="image" src="https://github.com/user-attachments/assets/0403db6e-7068-456d-9c0f-0d5506977abe" /> <img width="730" height="814" alt="image" src="https://github.com/user-attachments/assets/ec648650-5042-4772-92f9-7899a00fc715" />

This setup initializes the player character, handling spawning, dying, leaving, or any event that removes the character.
It loads animations, stores them in a table for easy access, and ensures all animations stop cleanly when the character resets.

💬 Notes

Taking a short break, but this project will be my main focus for a while — I want to fully demonstrate my skills and push what’s possible in Roblox Studio!

💡 Quick Fixes

Here are a few small typos you could clean up:

“veristile” → “versatile”

“interworking's” → “inner workings”

“mutiple” → “multiple”

“enviroment” → “environment”

“tollerable” → “tolerable”

“acsess” → “access”

“souley” → “solely”

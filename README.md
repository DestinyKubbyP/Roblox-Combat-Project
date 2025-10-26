# ⚔️ Roblox Combat Project

Hello! I'm **Peter**, an 18-year-old developer who’s been exploring **game development**, **reverse engineering**, and the **inner workings of computers** for a few years.  
This will be my **first in-depth public documentation** of my Roblox combat system — a project that combines everything I’ve learned so far. This project will be constantly updating and you will see it build on itself 🙂

---

## 🎯 Motivation & Vision

I’ve always wanted to create a **combat system that’s extremely versatile**, blending elements of **real-life combat** and **anime-style action**.  
My goal is to make a **true power fantasy** — a game where you can feel like you rule the world with your destructive power!

The system takes heavy inspiration from **Batman’s Free-Flow Combat** mechanics.

<img width="512" height="288" alt="gif" src="https://github.com/user-attachments/assets/fa2630a0-ec05-4389-9bea-b0a664c74958" />

---


This is the format for our ablities and how our input will be handled. This will make it easy for user customazation and other features.
<img width="458" height="149" alt="image" src="https://github.com/user-attachments/assets/21c3ced2-35f5-47aa-be10-ba9d5480e353" />

This will be the main loop for how our combat works.
<img width="916" height="706" alt="image" src="https://github.com/user-attachments/assets/04ecf535-f6af-485e-ba54-784f1ffa2bfa" />

This is how we handle all our inputs
<img width="1006" height="133" alt="image" src="https://github.com/user-attachments/assets/282cbb22-c321-4894-8d9c-6a13b31eaac6" />


## 🌀 Core Combat Features

- **Interactive environmental takedowns**
- **Multi-chain combo attacks**, letting players decide how hard they strike
- **Scalable special moves**, where you determine the amount of energy put into an attack
- **Randomized animation variants** to keep combat fluid and dynamic
- **Enemy-linked takedowns**, where dodging becomes a *dance of precision* that redirects enemy attacks into their allies
- **One-time auto-dodge mechanic** that instantly blocks and counters if timed correctly
- **Custom ragdoll physics**, deviating from Roblox’s default engine just for the challenge and fun of it!

---

## 🧩 Systems Overview

There will be many interconnected systems, so things may get complex at times.

- [Modules](#modules)
- [Animations](#animations)
- [Scripts](#scripts)

---

## ⚙️ Modules

### 🔦 Ray Module

<img width="1300" height="759" alt="image" src="https://github.com/user-attachments/assets/c8b1060f-ae2f-48dc-8aa5-4ae2603d5dfa" />

The **Ray Module** handles all raycasting logic.  
It will be used for:
- Detecting hit points (smash points, projectiles, etc.)
- Checking world positions relative to object bounds
- Supporting every major combat interaction  

This module is a *core component* of the entire combat system. :0

---

## 🧠 Scripts

### 💻 Client

All gameplay logic runs **entirely on the client**.  
The only communication with the server will be for **saving data** and **persistence tasks**.

---

#### 🧩 Variables & Environment

Our scripts, modules, and instance values are stored under **StarterPlayerScripts**.

<img width="830" height="188" alt="image" src="https://github.com/user-attachments/assets/3fc61471-6919-4f68-be8f-c4bde305b822" />

Maintaining this **ancestor-child structure** helps keep the project organized.  
For example, to access a player’s money value stored in a `Values` folder:

    script.Parent.Values.Money

This makes revisiting the code much easier after a break.  
You can see another example below where we access the `Rays` module:

<img width="1713" height="318" alt="image" src="https://github.com/user-attachments/assets/4a4bb83e-0e3b-4ed9-b5ee-cca900a368f6" />

We define key **objects** and **services** such as `Players` and `ReplicatedStorage` to access assets and organize workspace parts.  
This structure also helps us exclude certain objects from raycasts more easily.  

The mouse will be used for **interactive UI events**, which will be covered later.

---

#### 🧱 Types

<img width="1046" height="551" alt="image" src="https://github.com/user-attachments/assets/25d85710-8eb0-45b6-a699-07fcb925eb28" />

I usually don’t use **types** in Lua because of how loosely the language handles objects.  
The `type` `'casting'` mainly improves readability — it doesn’t add any safety like static casting in lower-level languages.  
You don’t need to worry about things like corrupting memory by casting a 2-byte value into a 4-byte slot.

<img width="672" height="194" alt="image" src="https://github.com/user-attachments/assets/0403db6e-7068-456d-9c0f-0d5506977abe" />

<img width="730" height="814" alt="image" src="https://github.com/user-attachments/assets/ec648650-5042-4772-92f9-7899a00fc715" />

This setup initializes the player character, handling spawning, dying, leaving, or any event that removes the character.  
It loads animations, stores them in a table for easy access, and ensures all animations stop cleanly when the character resets.

---

## 🛠️ Notes

Taking a short break, but this project will be my **main focus** for a while — I want to fully demonstrate my skills and push what’s possible in Roblox Studio!

---

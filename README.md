# ⚔️ Roblox Combat Project

Hello! I'm **Peter**, an 18-year-old developer passionate about **game development**, **reverse engineering**, and exploring the **inner workings of computers**.  
This project serves as my **first in-depth public documentation** of my custom Roblox Combat System — a culmination of everything I’ve learned so far.  
Expect continuous updates as the system evolves! :)

---

## 🎯 Motivation & Vision

I’ve always wanted to build a **combat system that’s both deeply versatile and cinematic**, combining the precision of **real-life combat mechanics** with the flair of **anime-style action**.  
My ultimate goal is to create a **true power fantasy** — a game where players feel unstoppable and in full control of their destructive abilities.

The system draws heavy inspiration from **Batman’s Free-Flow Combat** mechanics.

![Combat Inspiration](https://github.com/user-attachments/assets/fa2630a0-ec05-4389-9bea-b0a664c74958)

---

## 🌀 Core Combat Features

- **Interactive environmental takedowns** for cinematic finishes  
- **Multi-chain combo attacks**, allowing players to vary the intensity of strikes  
- **Scalable special moves** — control how much energy you channel into each attack  
- **Randomized animation variants** for natural and fluid combat flow  
- **Enemy-linked takedowns**, turning dodging into a precise, dance-like maneuver that redirects attacks  
- **Auto-dodge mechanic** that instantly blocks and counters when perfectly timed  
- **Custom ragdoll physics system**, built from scratch to go beyond Roblox’s default physics  

---

## 🧩 Systems Overview

This project consists of multiple interconnected systems, designed to work together seamlessly:

- [Modules](#modules)  
- [Animations](#animations)  
- [Scripts](#scripts)

---

## ⚙️ Modules

### 🔦 Ray Module

![Ray Module](https://github.com/user-attachments/assets/c8b1060f-ae2f-48dc-8aa5-4ae2603d5dfa)

The **Ray Module** is responsible for all raycasting logic.  
It will be used for:  
- Detecting hit points (smash points, projectiles, etc.)  
- Measuring object bounds and world positions  
- Handling all environment-aware combat calculations  

This is one of the **core building blocks** of the combat system.

---

## 🧠 Scripts

### 💻 Client

All gameplay logic runs **entirely on the client**.  
The only communication with the server will handle **saving data** and **long-term persistence**.

#### 🧩 Variables & Environment

All scripts, modules, and instance values are located under **StarterPlayerScripts** for easy access and organization.

![Script Layout](https://github.com/user-attachments/assets/3fc61471-6919-4f68-be8f-c4bde305b822)

This structure ensures clean navigation and maintainability.  
Example:

    script.Parent.Values.Money

Referencing a module (e.g., `Rays`):

![Rays Module Reference](https://github.com/user-attachments/assets/4a4bb83e-0e3b-4ed9-b5ee-cca900a368f6)

Essential **services** (e.g., `Players`, `ReplicatedStorage`) and workspace folders manage organization and raycast exclusions.  
Mouse input supports **interactive UI events**.

#### 🧱 Types

![Type Example 1](https://github.com/user-attachments/assets/25d85710-8eb0-45b6-a699-07fcb925eb28)
![Type Example 2](https://github.com/user-attachments/assets/0403db6e-7068-456d-9c0f-0d5506977abe)
![Type Example 3](https://github.com/user-attachments/assets/ec648650-5042-4772-92f9-7899a00fc715)

Lua types are mainly used for readability rather than strict safety — Lua’s flexibility allows safe handling of dynamic types.  
This system manages character initialization, event handling, and animation loading.

---

## 💥 Smash Wall & Ability Logic

Defines the **core structure** of abilities and input handling for easy customization and scalability.

![Smash Wall Example](https://github.com/user-attachments/assets/21c3ced2-35f5-47aa-be10-ba9d5480e353)

Each ability stores **activation time**, **status**, and **bound input key**, keeping it flexible.

![Ability System Example](https://github.com/user-attachments/assets/04ecf535-f6af-485e-ba54-784f1ffa2bfa)

Abilities like **Smash Wall** use precise raycasting for surface detection and realistic animations.  
Here’s a simplified visualization:
<iframe width="560" height="315" src="https://www.youtube.com/embed/k6ALSq14ojQ" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

These **interacting points** represent height limits for actions — above head height (red) means invalid smash range.  
Above-head actions like **grappling** and **lunging** are handled separately.

The system for effects needs to be made more modular for wall smash effects, sounds, the attraction point system and our tweening :)
---

# 🧱 Development Log — 10/28/2025 11:32 AM

I have finished the **main animation for the wall smashing**!
<iframe width="560" height="315" src="https://www.youtube.com/embed/IFqYgCBWidQ" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

The projectile system is next — it will handle hit detection, motion prediction, and collision impact reactions.

---

## 🛠️ Notes

Taking a short break, but this project will be my **main focus** moving forward.  
I’m fully committed to pushing Roblox’s engine to its limits and showing what’s possible with **technical precision and creativity**.

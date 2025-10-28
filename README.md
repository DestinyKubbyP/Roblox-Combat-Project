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

[![Visualization](https://img.youtube.com/vi/k6ALSq14ojQ/0.jpg)](https://youtu.be/k6ALSq14ojQ)

These **interacting points** represent height limits for actions — above head height (red) means invalid smash range.  
Above-head actions like **grappling** and **lunging** are handled separately.

The system for effects needs to be made more modular for wall smash effects, sounds, the attraction point system and our tweening :)
---

# 🧱 Development Log — 10/28/2025 11:32 AM

I have finished the **main animation for the wall smashing**!

[![Wall Smash Demo](https://img.youtube.com/vi/IFqYgCBWidQ/0.jpg)](https://youtu.be/IFqYgCBWidQ)

The projectile system is next — it will handle hit detection, motion prediction, and collision impact reactions.

🧱 Development Log — 10/28/2025 1:00 PM
🧩 Global Handling Overhaul

I’ve completely reworked how globals are defined and updated across the engine.
Roblox can be notoriously inconsistent when it comes to globalizing variables cleanly — especially when working across different environments and modules.

To solve this, I’ve introduced a typed global table (_G_table) that holds all key runtime data. This data is now synchronized with both _G and the current script environment using a helper function, ensuring all scripts have consistent access to runtime variables without repetitive declarations.
```lua
type _G_table = {
	current_wall: Instance?,
	current_smash_point: Vector3?,
	current_position: Vector3?,
	wall_normal: Vector3?,
	lplr_larm: Instance?,
	lplr_rarm: Instance?,
	lplr_lleg: Instance?,
	lplr_rleg: Instance?,
	lplr_char: Instance?,
	lplr_hrp: Instance?,
	character_data: character
}

local ldata: _G_table = {
	current_wall = nil,
	current_smash_point = Vector3.new(),
	current_position = Vector3.new(),
	wall_normal = Vector3.new(),
	lplr_larm = nil,
	lplr_rarm = nil,
	lplr_lleg = nil,
	lplr_rleg = nil,
	lplr_char = nil,
	lplr_hrp = nil,
	character_data = nil
}

_G.ldata = ldata

_G.update_globals = function()
	for i, v in pairs(ldata) do
		_G[i] = v
		getfenv(2)[i] = v -- Roblox’s environment scoping can be messy.
	end
end
```

This pattern makes it far easier to manage state between scripts without polluting the global space in unpredictable ways. It’s essentially a lightweight environment-syncing layer that acts like a “mini-engine” runtime controller.

⚙️ Character Initialization

I also refined the character initialization logic to fit this new global system. Everything from the player’s limbs to animation bindings now registers directly into _G, which is then unpacked for local accessibility.
```lua
local init_character = function(char: Model): nil
	lchar = {
		body_parts = get_baseparts(char),
		health = 100,
		dead = false,
		connections = {},
		animations = {},
		velocity = Vector3.new(),
		velocity_magnitude = 0,
		velocity_direction = Vector3.new(),
		velocity_normal = Vector3.new(),
		inital_launch_v = Vector3.new(),
		current_launch_v = Vector3.new(),
		self = char
	}

	ldata.lplr_char = char
	ldata.lplr_larm = char:WaitForChild("Left Arm")
	ldata.lplr_rarm = char:WaitForChild("Right Arm")
	ldata.lplr_lleg = char:WaitForChild("Left Leg")
	ldata.lplr_rleg = char:WaitForChild("Right Leg")
	ldata.lplr_hrp = char:WaitForChild("HumanoidRootPart")
	ldata.character_data = lchar

	_G.update_globals()

	local character_animations = {}

	repeat task.wait() until lchar.self:FindFirstChild("Humanoid")

	for i, v in pairs(animation_ids) do
		local animation = create_instance("Animation", {
			AnimationId = "rbxassetid://" .. tostring(v),
			Name = i,
			Parent = char
		})

		character_animations[i] = lchar.self.Humanoid:LoadAnimation(animation)
	end

	lchar.animations = character_animations
end
```
💡 Summary

This new global management system:

Greatly improves readability and maintainability

Keeps the type system in sync for better code hinting and IntelliSense

Reduces redundancy across scripts

Provides a foundation for a true client runtime layer, almost like a tiny game engine inside Roblox

Roblox might fight against structured global usage, but that’s part of what makes building systems like this so fun. The more we tame the environment, the more ours the engine becomes — and that’s exactly what this project is about.

More environment handling tweaks coming soon 👀
---

## 🛠️ Notes

Taking a short break, but this project will be my **main focus** moving forward.  
I’m fully committed to pushing Roblox’s engine to its limits and showing what’s possible with **technical precision and creativity**.

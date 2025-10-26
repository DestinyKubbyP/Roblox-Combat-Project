# ⚔️ Roblox Combat Project

Hello! I'm **Peter**, an 18-year-old developer passionate about **game development**, **reverse engineering**, and exploring the **inner workings of computers**.  
This project serves as my **first in-depth public documentation** of my custom Roblox Combat System — a culmination of everything I’ve learned so far.  
Expect continuous updates as the system evolves! :)

---

## 🎯 Motivation & Vision

I’ve always wanted to build a **combat system that’s both deeply versatile and cinematic**, combining the precision of **real-life combat mechanics** with the flair of **anime-style action**.  
My ultimate goal is to create a **true power fantasy** — a game where players feel unstoppable and in full control of their destructive abilities.

The system draws heavy inspiration from **Batman’s Free-Flow Combat** mechanics.

<img width="512" height="288" alt="gif" src="https://github.com/user-attachments/assets/fa2630a0-ec05-4389-9bea-b0a664c74958" />

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

<img width="1300" height="759" alt="image" src="https://github.com/user-attachments/assets/c8b1060f-ae2f-48dc-8aa5-4ae2603d5dfa" />

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

---

#### 🧩 Variables & Environment

All scripts, modules, and instance values are located under **StarterPlayerScripts** for easy access and organization.

<img width="830" height="188" alt="image" src="https://github.com/user-attachments/assets/3fc61471-6919-4f68-be8f-c4bde305b822" />

This structure ensures clean navigation and maintainability.  
For example, to access a player’s money value stored in a `Values` folder:

    script.Parent.Values.Money

You can also see another example below when referencing the `Rays` module:

<img width="1713" height="318" alt="image" src="https://github.com/user-attachments/assets/4a4bb83e-0e3b-4ed9-b5ee-cca900a368f6" />

We define essential **services** (e.g., `Players`, `ReplicatedStorage`) and workspace folders to manage organization and object exclusion during raycasts.  
Additionally, mouse input will be used for **interactive UI events**, covered later on.

---

#### 🧱 Types

<img width="1046" height="551" alt="image" src="https://github.com/user-attachments/assets/25d85710-8eb0-45b6-a699-07fcb925eb28" />

I generally don’t rely heavily on **types** in Lua because of the language’s flexible handling of objects.  
Here, the use of `type` mainly serves readability rather than strict type safety — since Lua doesn’t risk memory corruption from improper casting.

<img width="672" height="194" alt="image" src="https://github.com/user-attachments/assets/0403db6e-7068-456d-9c0f-0d5506977abe" />

<img width="730" height="814" alt="image" src="https://github.com/user-attachments/assets/ec648650-5042-4772-92f9-7899a00fc715" />

This setup manages player character initialization, handling events like spawning, death, and cleanup.  
Animations are loaded and stored in a centralized table for quick access, and safely disconnected when the character resets.

---

#### 💥 Smash Wall & Basic Concepts

This defines the core structure of abilities and input handling, designed for easy customization and modular expansion.

<img width="458" height="149" alt="image" src="https://github.com/user-attachments/assets/21c3ced2-35f5-47aa-be10-ba9d5480e353" />

Each ability stores key data such as **last activation time**, **active status**, and **bound input key**.  
This format keeps the system flexible and scalable.

<img width="916" height="706" alt="image" src="https://github.com/user-attachments/assets/04ecf535-f6af-485e-ba54-784f1ffa2bfa" />

The example above shows how the **Smash Wall** ability detects interactable surfaces.  
It fires a ray from the camera’s direction, checks distance thresholds, and assigns the detected wall as the target for the smash sequence.

This ensures realism — players won’t float or clip mid-air when activating the ability.  

Once triggered, the ability executes a cinematic sequence involving debris, physics impulses, and layered animations.  
The player fades, teleports, and performs synchronized animations that blend seamlessly with the environment.

<img width="1006" height="133" alt="image" src="https://github.com/user-attachments/assets/65dbaf9c-9b5d-4f66-910f-655408f4472c" />

This is how the inputs are handled. With the formated ability table. It sees if the keys are even used in general and checks if the key pressed is a ability bind and activates the ability based off its activate function. This will work for all abiltites and moves. 


These are the functions used for smash wall and I will go more in depth : 

```lua

local interacting_point_vis_min = create_instance("Part",{
	Parent = workspace,
	Color = Color3.fromRGB(255,0,0),
	Anchored = true,
	Material = Enum.Material.Neon,
	Size = Vector3.new(1,1,1),
	CanCollide = false
})

local interacting_point_vis_max = create_instance("Part",{
	Parent = workspace,
	Color = Color3.fromRGB(255,0,0),
	Anchored = true,
	Material = Enum.Material.Neon,
	Size = Vector3.new(1,1,1),
	CanCollide = false
})

---combat functions

local used_keys = {"E"}
local smash_wall;
local q_events = {}
local current_wall,current_smash_point,current_position,wall_normal
local active_abilities : abilties;

local character_fade = function(new_cords : CFrame) 
	local original_cf = lchar.body_parts.HumanoidRootPart.CFrame

	for i,v in pairs(lchar.body_parts) do 
		local part = v:Clone()
		part.Parent = workspace
		part.Transparency = 0.7
		part.CanCollide = false
		part.Anchored = true
		part.Size = v.Size 
		part.CFrame = v.CFrame
		part.Material = Enum.Material.ForceField
		part.Color = Color3.fromRGB(0,124,233)
		game:GetService("Debris"):AddItem(part,0.2)
	end
end

local lowest_surface_from_pos = function(position : CFrame) : Vector3
	if not lchar then return end
	local root = lchar.body_parts.HumanoidRootPart
	local results = ray_casting.shoot_ray({
		distance = -1000,
		orig = position.Position,
		dir = root.CFrame.UpVector,
		ignores = {lchar.self},
	})

	return results and results.Position
end

smash_wall = function() : boolean
	print("Activated")
	if lchar.dead then return false end
	local distance = (current_position - current_smash_point).Magnitude
	if distance > 5 then return false end
	local current_wall = current_wall
	local current_smash_point = current_smash_point;
	local wall_normal = wall_normal
	local root = lchar.body_parts.HumanoidRootPart
	local left_arm = lchar.body_parts["Left Arm"]
	local new_cf = CFrame.new(current_smash_point + (wall_normal * 1.5),current_smash_point + (wall_normal * 5))
	local unit = new_cf - new_cf.Position
	local arm_tip = (left_arm.CFrame + (left_arm.CFrame.UpVector * Vector3.new(1,left_arm.Size.Y / 2,0))).Position
	local smash_arm_diff = ((arm_tip - current_smash_point) * Vector3.new(1,0,1)).Magnitude
	local size_add = (root.Size.Y / 2) + (lchar.body_parts["Left Leg"].Size.Y)


	character_fade() 
	wait(0.1)
	new_cf = new_cf + (unit.RightVector * (3))
	root.CFrame = (CFrame.new(lowest_surface_from_pos(new_cf)) * new_cf.Rotation) + Vector3.new(0,size_add,0)
	root.Anchored = true
	active_abilities.wall_smash.last_active = tick()
	local wall_smash_anim = lchar.animations.SmashWall;
	local grab_rock_anim = lchar.animations.GrabRock
	local throw_anim = lchar.animations.Throw

	local start = tick()
	local throw_start = tick()
	local tossed_to_other = false;
	print(string.rep("=",7).."Action Event"..string.rep("=",7))
	local main_rock;


	local animation_handling : thread; animation_handling = task.spawn(function()
		wall_smash_anim.Priority = Enum.AnimationPriority.Action4
		wall_smash_anim:Play()
		task.wait(0.05)


		for i = 1,10 do 
			local rock = Instance.new("Part",workspace)
			rock.Size = Vector3.new(0.4,0.4,0.4)
			rock.Anchored = false 
			rock.Material = current_wall.Material
			rock.Color = current_wall.Color
			rock.Parent = workspace
			rock.CFrame = CFrame.new(current_smash_point) + (wall_normal * 3)
			rock.Velocity = Vector3.new(math.random(-10,10),math.random(10,20),math.random(-10,10))
		end
		
		local larm = lchar.body_parts["Left Arm"];
		local rock = replicated.Assets.Rock:Clone()
		local grab_cf = CFrame.new(larm.LeftGripAttachment.WorldCFrame.Position,larm.LeftGripAttachment.WorldCFrame.Position + (wall_normal * 4))
		rock.Parent = workspace
		rock.Anchored = true
		rock.CFrame = CFrame.new(current_smash_point) + (wall_normal * 1.6)
		rock.Material = current_wall.Material
		rock.Color = current_wall.Color
		rock.ParticleEmitter.Acceleration = grab_cf.LookVector
		rock.ParticleEmitter.Color = ColorSequence.new(rock.Color)
		main_rock = rock
		main_rock.SmashSound:Play()
	
		local cracked_wall = replicated.Assets.Crack:Clone()
		cracked_wall.Parent = workspace
		cracked_wall.Size = Vector3.new(1,1,0.1)
		cracked_wall.Transparency = 1
		cracked_wall.Anchored = true
		cracked_wall.CFrame = CFrame.new(current_smash_point,current_smash_point + (wall_normal * 5))

		
		repeat task.wait() until wall_smash_anim.IsPlaying == false;
		print("Smashed Wall!")
	
	
	
		if rock and rock:FindFirstChild("ParticleEmitter") then 	
			rock.ParticleEmitter.Enabled = false
		end 
		
		grab_rock_anim.Priority = Enum.AnimationPriority.Action4
		grab_rock_anim:Play()
		repeat task.wait() until grab_rock_anim.IsPlaying == false 
		rock.Anchored = false 
		rock.CFrame = larm.LeftGripAttachment.WorldCFrame
		local a1 = Instance.new("WeldConstraint",rock)
		a1.Part0 = larm 
		a1.Part1 = rock
		
		print("Grabbed Rock!")
		throw_start = tick()
		throw_anim.Priority = Enum.AnimationPriority.Action4
		throw_anim:Play()
		return;
	end)

	local event_handling : thread; event_handling = task.spawn(function()
		while task.wait() do 
			local diff = (tick() - throw_start)


			if diff >= time_frames.Throw.toss_rock.end_t and not tossed_to_other then 
				print("Toss to right")
				tossed_to_other = true;
			end


			if diff >= time_frames.Throw.release_rock.end_t and throw_anim.IsPlaying then 
				print("Throw!")
				
				lchar.body_parts.HumanoidRootPart.Anchored = false
				
				if main_rock then 
					main_rock:Destroy()
					main_rock = nil
				end
				
				return;
			end
		end

		return
	end)

	return true
end
```

---

There’s a lot happening under the hood in these systems — from **timing-based animation sequencing** to **custom physics manipulation**.  
I’ll continue walking through each part of the combat pipeline soon and explain the design logic in depth.

---

## 🛠️ Notes

Taking a short break for now, but this project will be my **main focus** going forward.  
I’m fully committed to pushing Roblox’s engine to its limits and showcasing what’s possible with creativity and technical precision.

---

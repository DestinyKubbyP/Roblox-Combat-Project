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

#### Smash Wall and Basic Concepts


This is the format for our ablities and how our input will be handled. This will make it easy for user customazation and other features. 
<img width="458" height="149" alt="image" src="https://github.com/user-attachments/assets/21c3ced2-35f5-47aa-be10-ba9d5480e353" />

It will have specfic information to the action and will store usful infromation like when it was last active, currently active, and the sepcfied bind used to activate such ability.

This will be the main loop for how our combat works. For now it only handles our smash wall. It shoots a ray via the direction and origin of the camera. If it hits a interactable wall within the distance of 300 meters it will set the data for our smash wall function. We also display the minium and maximum height of the smash. The intersecting_point_vis_min is used as a max height for our smash wall. It wouldnt make sense if we teleported in the air levitating smash a wall and then procced to throw a rock. 

<img width="916" height="706" alt="image" src="https://github.com/user-attachments/assets/04ecf535-f6af-485e-ba54-784f1ffa2bfa" />


You remember how we set our globals earlier in our main loop for our combat. Well this is where they will be used. Once the bind is pressed which would be E.

<img width="1006" height="133" alt="image" src="https://github.com/user-attachments/assets/282cbb22-c321-4894-8d9c-6a13b31eaac6" />

This is the function that is assigned to the activate function.

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

There is a lot to these functions I provided, but I will give examples and walk you through it in a way in which its not overly complicated. I will try my best to present it in a way that my brain processes it. So maybe you can link my brains behavoir to your own. ---continue later

## 🛠️ Notes

Taking a short break, but this project will be my **main focus** for a while — I want to fully demonstrate my skills and push what’s possible in Roblox Studio!

---

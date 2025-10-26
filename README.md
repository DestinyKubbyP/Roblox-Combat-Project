# Roblox-Combat-Project

Hello, I'm Peter and I am 18 years old and I've been working on game development, reversing, and learning the interworking's of computers for a couple years and haven't documented much. So, this will be my first public in-depth documentation of the interworking's of my Roblox Combat System :)

I've always wanted to create a combat system that is extremly veristile and has elements of real life combat and anime style combat. I want to make a game that is a true power fantasy where you can feel like you rule the world with your destructive power!!

The combat system will be heavily inspired off of batmans free flow combat

<img width="512" height="288" alt="gif" src="https://github.com/user-attachments/assets/fa2630a0-ec05-4389-9bea-b0a664c74958" />

The combat system will feature interactive environmental takedowns and multi-chain combo attacks, giving players freedom to decide how hard they strike.
Every special move is scalable, meaning the player determines how much energy or force is put into an attack.
To keep combat feeling fluid and dynamic, each move will have randomized animation variants and context-based finishers.

I also plan to include enemy-linked takedowns, where dodging and movement feel like a dance of precision — redirecting enemy attacks into their allies.

Additionally, I aim to experiment with realistic ragdoll physics, building a custom system that deviates from Roblox’s default physics engine — just for the challenge and fun of creating something unique. 

The key feature I want to add to the system is a one time auto dodge based action. Where if you dont dodge the attack you block and redirect the attack as fast as lighting.

### Systems

There will be a ton of different systems that are intertwined so it can get a bit confusing at times.


- [Modules](#Modules)
- [Animations](#usage)

## Modules

### Ray Module
<img width="1300" height="759" alt="image" src="https://github.com/user-attachments/assets/c8b1060f-ae2f-48dc-8aa5-4ae2603d5dfa" />

We will have a few key modules firstly is our raycast module. This will hold our simple casting function for now. We will be using this for mutiple things like casting smashpoint, projectiles, and checking world positions relative to other objects bounding size. It will be the major component in pretty much every key aspect of our combat system :0


## Scripts 

### Client

Our client is where everything will be handled. This game I'm making will souley be handled on the client meaning I don't have to do any form of communication with the server. The only communciation I will be making with the server is for game saves and other processes that may require data storing.

#### Variables and Enviroment
First lets talk about the structure of our enviroment and variables that we need to define.
First our scripts, modules, and instance values will be stored under the starter player scripts

<img width="830" height="188" alt="image" src="https://github.com/user-attachments/assets/3fc61471-6919-4f68-be8f-c4bde305b822" />

We will need to remember this ancestor child dynamic because we will be navigating through this enviroment quite frequently. For example if we stored our money value in a instance value in the values folder and we were located in our clients script enviroment we would do. "script.Parent.Values.Money" this makes things more organized and tollerable to come back to after a long break. You can see another example of this in the image below when we acsess our "Rays" module :)

<img width="1713" height="318" alt="image" src="https://github.com/user-attachments/assets/4a4bb83e-0e3b-4ed9-b5ee-cca900a368f6" />

We define key objects and services needed to carry out coding. We grab the Players and ReplicatedStorage path ways so we can acsess certain assets and objects. We have a couple folders in our workspace that we help organize part creation. This also makes it easier to exclude certain objects from casting. 

We will be using our mouse for some interactable UI events which we will get into a lot later on.

#### Types
<img width="1046" height="551" alt="image" src="https://github.com/user-attachments/assets/25d85710-8eb0-45b6-a699-07fcb925eb28" />

I usally don't use types in Lua because I don't really enjoy how they handle objects in general. Its a lot more less defined and loose. type in lua is pretty usless it just makes the code more readable to me and you. You don't have to worry about corrupting memory by casting types improperly.

<img width="672" height="194" alt="image" src="https://github.com/user-attachments/assets/0403db6e-7068-456d-9c0f-0d5506977abe" />

<img width="730" height="814" alt="image" src="https://github.com/user-attachments/assets/ec648650-5042-4772-92f9-7899a00fc715" />

This will be setting up our main character and handle the initalization of the player spawning in ,dying, leaving or any other event that causes character removal or initalization. This will setup the basic framework for our character. On character intialziation we create the dictonary of data and load all animations and store the animation tracks in a table so they are easily assesable. All deinitalizing does is stop all running animations and disconnect any active connections.


-----------------------------Taking a break and will continue on this project tmrw :)

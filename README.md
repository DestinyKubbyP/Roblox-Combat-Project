# Roblox-Combat-Project

Hello, I'm Peter and I am 18 years old and I've been working on game development, reversing, and learning the interworking's of computers for a couple years and haven't documented much. So, this will be my first public in-depth documentation of the interworking's of my Roblox Combat System :)

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

First lets talk about the structure of our enviroment and variables that we need to define.
First our scripts, modules, and instance values will be stored under the starter player scripts

<img width="830" height="188" alt="image" src="https://github.com/user-attachments/assets/3fc61471-6919-4f68-be8f-c4bde305b822" />

We will need to remember this ancestor child dynamic because we will be navigating through this enviroment quite frequently. For example if we stored our money value in a instance value in the values folder and we were located in our clients script enviroment we would do. "script.Parent.Values.Money" this makes things more organized and tollerable to come back to after a long break.

<img width="1713" height="318" alt="image" src="https://github.com/user-attachments/assets/4a4bb83e-0e3b-4ed9-b5ee-cca900a368f6" />

We define key objects and services needed to carry out coding. We grab the Players and ReplicatedStorage path ways so we can acsess certain assets and objects. We have a couple folders in our workspace that we help organize part creation. This also makes it easier to exclude certain objects from casting. 

We will be using our mouse for some interactable UI events which we will get into a lot later on.

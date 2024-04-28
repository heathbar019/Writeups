# b01lers CTF - Chest Mix (miscellaneous, 405p, 60 solves) 4/18/2024

> Here's a minecraft world, the flag is on a piece of paper in the the chest by spawn, it's hard to miss. Version 1.20
> 
> By colbyjack

## Preface
This is a fun challenge involving a minecraft world and exploring it's binary files! To solve this challenge I needed a tool called NBTExplorer as well access to minecraft version 1.20, so make sure you have these if you want to follow along. If you do not own minecraft or do not have it installed, but still want to try to solve this challenge, I will discuss using an alternative [tool](https://unmined.net/downloads/) later in the solution.

## Solving
![image](https://github.com/heathbar019/Writeups/assets/114100890/d14ae1ee-853a-4bfc-9e07-e8827ba9b560)
![image](https://github.com/heathbar019/Writeups/assets/114100890/70e24912-75b2-40f9-8f0f-5846694cd4a3)

We start with a zip file called chest-mix.zip, and based off what we are told, we can assume this is probably a folder for a minecraft map/world. Quickly unzipping it and looking inside will confirm this, as we can see exactly what we would expect to see inside a minecraft map folder (advancements, data, entities, playerdata, region, level.dat, etc.) My first thought was to simply load up the world in minecraft and see what were working with. Maybe it will be as easy as just grabbing a piece of paper from a chest near spawn? But probably not...

![chestmix1](https://github.com/heathbar019/Writeups/assets/114100890/81f6edc1-c8c0-4be4-92d6-64372e396d12)

So I spawn in, look around, and yes, of course there is an absolutely ginormous terracotta chest. I wanted to change my gamemode to creative so I could get a better look around and inside the chest, but the world had cheats turned off. Unfortunately, the only to turn cheats back on after creating a minecraft world is to manually edit the game files. So for the time being, I went up to the chest and punched out a few blocks to try and get a better look at the inside.

![image](https://github.com/heathbar019/Writeups/assets/114100890/140aa10b-15c7-45d8-b4de-795412db31cb)
![image](https://github.com/heathbar019/Writeups/assets/114100890/c2438897-4c69-4c02-97e4-cc8ca6767178)


Peeking inside reveals some actual chests, and from what I can tell, there is a lot of them. Judging by the way my game drops to three frames per second whenever I am near and/or facing the giant chest structure, it's safe to assume that this entire giant chest is filled with smaller real chests, and all of the block entities are degrading my game's performance. Looking inside these chests we can see that they are full of single pieces of paper, some of which are renamed to "Nope, not in this chest :D". At this point, the actual nature of this challenge becomes clear. We will have to somehow devise a way to search through every paper in every single chest, and determine which one has the flag written on it.

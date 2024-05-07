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

So I spawn in, look around, and yes, *of course* there is an absolutely ginormous terracotta chest. I wanted to change my gamemode to creative so I could get a better look around and inside the chest, but the world had cheats turned off. Unfortunately, the only way to turn cheats back on after creating a minecraft world is to manually edit the game files. So for the time being, I went up to the chest and punched out a few blocks to try and get a better look at the inside.

![image](https://github.com/heathbar019/Writeups/assets/114100890/140aa10b-15c7-45d8-b4de-795412db31cb)
![image](https://github.com/heathbar019/Writeups/assets/114100890/c2438897-4c69-4c02-97e4-cc8ca6767178)


Peeking inside reveals some actual chests, and from what I can tell, there is a lot of them. Judging by the way my game drops to three frames per second whenever I am near and/or facing the giant chest structure, it's safe to assume that this entire giant chest is filled with smaller real chests, and all of the block entities are degrading my game's performance. Looking inside these chests we can see that they are full of pieces of paper, some of which are renamed to "Nope, not in this chest :D". At this point, the actual nature of this challenge becomes clear. We will have to somehow devise a way to search through every paper in every single chest, and determine which one has the flag written on it.

From here I did some research online into tools that are capable of exploring and manipulating minecraft's game files, and I came across something called NBTExplorer. The Named Binary Tag (NBT) is a tree data structure that Minecraft uses in it's save files to store data, and this tool is specifically made for reading and editing those files as well as Minecraft's various other file types (.mcr, .mca). Before anything, I want to enable cheats in this save file so that I can enter creative mode and confirm my suspicions about there being a lot of chests. Now that we have this editor, this should be possible by finding the level.dat file and editing the game rule "allowCommands" to be equal to 1.

![image](https://github.com/heathbar019/Writeups/assets/114100890/9d0a2af4-0dbf-4b6a-8f54-1bbaf3b1dc30)
![image](https://github.com/heathbar019/Writeups/assets/114100890/07d82a1b-45fa-4bcf-94cd-df1adc558300)

When I load back into the world, I should now be able to use commands to enter creative mode and confirm my beliefs about the chest by flying up aboveto see everything better.

![image](https://github.com/heathbar019/Writeups/assets/114100890/f0d38a4a-0b42-4f98-99bc-e095a1ed3639)
![image](https://github.com/heathbar019/Writeups/assets/114100890/f59f31ae-b5b9-4dd5-a36c-0123518e3199)



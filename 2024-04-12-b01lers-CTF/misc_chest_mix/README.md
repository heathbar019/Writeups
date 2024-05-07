# b01lers CTF - Chest Mix (miscellaneous, 405p, 60 solves) 4/18/2024

> Here's a Minecraft world, the flag is on a piece of paper in the chest by spawn, it's hard to miss. Version 1.20
> 
> By colbyjack

## Preface
This is a fun challenge involving a Minecraft world and exploring its binary files! To solve this challenge I needed a tool called NBTExplorer as well access to Minecraft version 1.20, so make sure you have these if you want to follow along. If you do not own Minecraft or do not have it installed, but still want to try to solve this challenge, I will discuss using an alternative tool [later](#solving-without-minecraft) in the writeup.

## Solving
![image](https://github.com/heathbar019/Writeups/assets/114100890/d14ae1ee-853a-4bfc-9e07-e8827ba9b560)
![image](https://github.com/heathbar019/Writeups/assets/114100890/70e24912-75b2-40f9-8f0f-5846694cd4a3)

We start with a zip file called chest-mix.zip, and based on what we are told, we can assume this is probably a folder for a Minecraft map/world. Quickly unzipping it and looking inside will confirm this, as we can see exactly what we would expect to see inside a Minecraft map folder (advancements, data, entities, playerdata, region, level.dat, etc.) My first thought was to simply load up the world in Minecraft and see what were working with. Maybe it will be as easy as just grabbing a piece of paper from a chest near spawn? But probably not...

![chestmix1](https://github.com/heathbar019/Writeups/assets/114100890/81f6edc1-c8c0-4be4-92d6-64372e396d12)

So I spawn in, look around, and yes, *of course* there is an absolutely ginormous terracotta chest. I wanted to change my gamemode to creative so I could get a better look around and inside the chest, but the world had cheats turned off. Unfortunately, the only way to turn cheats back on after creating a Minecraft world is to manually edit the game files. So for the time being, I went up to the chest and punched out a few blocks to try and get a better look at the inside.

![image](https://github.com/heathbar019/Writeups/assets/114100890/140aa10b-15c7-45d8-b4de-795412db31cb)
![image](https://github.com/heathbar019/Writeups/assets/114100890/c2438897-4c69-4c02-97e4-cc8ca6767178)


Peeking inside reveals some actual chests, and from what I can tell, there are a lot of them. Judging by the way my game drops to three frames per second whenever I am near and/or facing the giant chest structure, it's safe to assume that this entire giant chest is filled with smaller real chests, and all of the block entities are degrading my game's performance. Looking inside these chests we can see that they are full of pieces of paper, some of which are renamed to "Nope, not in this chest :D". At this point, the actual nature of this challenge becomes clear. We will have to somehow devise a way to search through every paper in every single chest and determine which one has the flag written on it.

From here I did some research online into tools that are capable of exploring and manipulating Minecraft's game files, and I came across something called NBTExplorer. The Named Binary Tag (NBT) is a tree data structure that Minecraft uses in its save files to store data, and this tool is specifically made for reading and editing those files as well as Minecraft's various other file types (.mcr, .mca). Before anything, I want to enable cheats in this save file so that I can enter creative mode and confirm my suspicions about there being a lot of chests. Now that we have this editor, this should be possible by finding the level.dat file and editing the game rule "allowCommands" to be equal to 1.

![image](https://github.com/heathbar019/Writeups/assets/114100890/9d0a2af4-0dbf-4b6a-8f54-1bbaf3b1dc30)
![image](https://github.com/heathbar019/Writeups/assets/114100890/07d82a1b-45fa-4bcf-94cd-df1adc558300)

When I load back into the world, I should now be able to use commands to enter creative mode and confirm my beliefs about the chest by flying up above to see everything better.

![image](https://github.com/heathbar019/Writeups/assets/114100890/f0d38a4a-0b42-4f98-99bc-e095a1ed3639)
![image](https://github.com/heathbar019/Writeups/assets/114100890/f59f31ae-b5b9-4dd5-a36c-0123518e3199)

With my previous suspicions now confirmed, I started to do more research into the Minecraft save file system to see how and where the type of data were looking for is actually stored. What I learned is that the entire Minecraft world is split up into "regions", and these regions are made up of a 32x32 area of "chunks". Furthermore, these chunks are made up of a 16x16 area of blocks. Since the chests containing the papers we want to be searching are blocks, or technically "block_entities", we will need to be looking into the region/chunk files. These files can be found within a subfolder of the world directory called "region.

![image](https://github.com/heathbar019/Writeups/assets/114100890/afd9577c-5be8-48b0-bf86-d77885338357)

Within the region folder, we can .mca files (Minecraft Anvile file format) starting with 'r' and then some coordinates relative to the world spawn. Each of these directories is a region that's been generated in the world, and within each region, we can see the corresponding chunks. While we could just try to use the search tool to search each region for an item value containing the flag format "bctf", performing such a wide search would possibly take hours to days to complete. Instead, if we can figure out the coordinates of where the large chest structure is located, we can narrow down our search to only the necessary chunks that contain chests. So I went back into the game and wrote down the coordinates while standing at each corner of the chest.

![image](https://github.com/heathbar019/Writeups/assets/114100890/09da78a2-11ec-4932-a445-22c60ca7e6e1)

The Y coordinates in this situation are unimportant since chunks extend from the bottom of the world to the top. With the knowledge that chunk 0,0 extends from block 0,0 to block 15,15 (16x16 blocks), we can calculate the chunks that we need to search from these coordinates. From the most negative chunk coordinate to the most positive, we need to search all chunks from -1,-8 to 2, -4. These chunks should be located within regions -1,-1 and 0,-1. From here we can select a chunk, click search, click find, and then specify the value of an item name that will include the flag format "bctf".

![image](https://github.com/heathbar019/Writeups/assets/114100890/e0da68f2-e4be-4e83-b762-3c54fb579807)
![image](https://github.com/heathbar019/Writeups/assets/114100890/fedf21c8-5e60-43e8-94ca-2b383b64bb5d)

Searching an individual chunk should take around 3 minutes or less. After a handful of searches, I was able to locate the paper containing the flag within chunk 1,-5.

![image](https://github.com/heathbar019/Writeups/assets/114100890/98faf17b-35ad-418a-9f75-552c1bb269e5)

The flag is "bctf{ch1st_ch2st_ch3st_ch4st_ch5st}"!

## Extra

Just for fun, I wanted to see if I could find the piece of paper with the flag on it within the actual game. If we scroll down a tiny bit from where the flag was, we can find information on the actual chest block that contains the paper, specifically its coordinates.

![image](https://github.com/heathbar019/Writeups/assets/114100890/e1497565-ef04-427e-8aee-21c3a3037e78)

Now, all I have to do is find that chest in the game.

![image](https://github.com/heathbar019/Writeups/assets/114100890/35434173-4dda-4c8f-8125-0bde2c3aabf1)

There it is!

### Solving Without Minecraft

I wanted to also include a method to solve this challenge without using Minecraft since not everyone may own a copy of the game. All the information from Minecraft that you need to solve this challenge within a reasonable time is the coordinates of the chest structure, and perhaps some kind of way to visualize the world without the game itself. Searching for a tool that could do this online, I came across a free application called [uNmINeD](https://unmined.net/downloads/). uNmINeD is an easy-to-use and fast Minecraft world viewer and mapper tool that reads Minecraft Java and Bedrock world files. Uploading the save into this application gives a top-down visualization of the world, and even provides the coordinates for blocks, chunks, and regions. If I had used this tool from the start I could have skipped the annoying calculations!

![image](https://github.com/heathbar019/Writeups/assets/114100890/d7c7fb31-2853-40f9-9f25-d6a54b735836)

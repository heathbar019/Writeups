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

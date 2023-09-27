# vsCTF - Canguard? (forensics, 460p, 53 solves) 9/23/23

> The user of this PC was caught cheating on Valorant, can you figure out why his cheat was detected?

We are provided with two downloads, one with a .ad1 extension and another with a .ad2 extension. Initially I assumed we were dealing with two different formats for logical image files and that the files were images of the same drive that were both provided just to give more options to use. Later, while trying to replicate this solve I found out that I was wrong, but I'll talk about that more later. For now I'll only investigate the .ad1 file and if I reach a dead end then I'll guess my assumption was wrong and start investigating the .ad2 file. After a quick google search I find out that .ad1 files can be opened with FTK imager, so I'll start there.
![image](https://github.com/heathbar019/Writeups/assets/114100890/80aca858-4294-411a-af2d-5a09983e0986)

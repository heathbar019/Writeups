# Spooky CTF - abducted (forensics, 399p, 94 solves) 11/2/2023

> I was going to just give you this flag, but the alien mothership downloaded it and deleted it from my computer!
> We managed to get a snapshot of their mothership before it got away but it's in some weird proprietary format, can you find the flag they stole?

This challenge gives us a link to a download for a .ad1 disk image file. I have some experience with this file type so I know where to start when it comes to handling them. If you'd like to know more about .ad1 files, I go into detail about them in this writeup from VSCTF 2023. But for now, all we need to know is that .ad1 files are a proprietary disk image file that can only be directly opened by FTK imager. So that is what we will be using! 

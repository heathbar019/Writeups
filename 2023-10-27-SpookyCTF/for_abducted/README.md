# Spooky CTF - abducted (forensics, 399p, 94 solves) 11/2/2023

> I was going to just give you this flag, but the alien mothership downloaded it and deleted it from my computer!
> We managed to get a snapshot of their mothership before it got away but it's in some weird proprietary format, can you find the flag they stole?

This challenge gives us a link to a download for a .ad1 disk image file. I have some experience with this file type so I know where to start when it comes to handling them. If you'd like to know more about .ad1 files, I go into detail about them in [this writeup](https://github.com/heathbar019/Writeups/tree/main/2023-09-23-VSCTF/for_canguard#additional-notes-and-research) from VSCTF 2023. But for now, all we need to know is that .ad1 files are a proprietary disk image file that can only be directly opened by FTK imager. So that is what we will be using! Once FTK imager is open, go to File > Add Evidence Item, and select Image File. Now we just select the file path and we will be able to look through our disk image.

![image](https://github.com/heathbar019/Writeups/assets/114100890/a356e57f-770a-4519-9d09-158c58fd1c1c)

Judging by the file structure, we can assume this is a Windows computer that we are dealing with. Since we aren't directed anywhere specific by the challenge, let's start by looking at the Downloads, Desktop, and Documents folders and see if we can find anything there. If we open up the Users folder we see a user named "mothership". That is definitely where we want to look.

![image](https://github.com/heathbar019/Writeups/assets/114100890/873a547f-2756-4448-a55b-d0cd4160ca48)

If we check within this user's desktop and documents we find that they are empty, however, we find the following "flag.jpg" file within the downloads!

![image](https://github.com/heathbar019/Writeups/assets/114100890/f562c4e0-b4d3-4375-8076-0a445be22725)

Despite the preview cutting off the image, we do have the full-length flag here! We simply have to scroll through and we can get the following flag to submit; "NICC{15_TH15_C0N51D3R3D_4_R3C4PTUR3D_FL4G_N0W?}".

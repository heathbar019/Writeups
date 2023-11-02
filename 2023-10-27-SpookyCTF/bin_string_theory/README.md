# Spooky CTF - String Theory (binary, 50p, 270 solves) 11/2/2023

> I received this very weird email that only contained a file called "openme", it looks like an executable.
> Can you see if there is anything weird in this file?

In this challenge, we are given a single file called "openme". Given the challenge category, this is probably an ELF file or some similar binary format. We can check just in case by running the "file" command like so.

![image](https://github.com/heathbar019/Writeups/assets/114100890/eebc80f3-30c4-4d7c-9337-948946f2cff6)

This confirms that we are indeed working with an ELF file type. Next, we can execute this file and observe its functionality by running "./openme".

![image](https://github.com/heathbar019/Writeups/assets/114100890/567d4096-c3d1-4f03-bc60-71054e39baad)

The program seems to prompt the user for some text and terminates. There are many different ways to go about figuring things out from here, however, given the challenge name, it might be as simple as running the strings command on this file. So let's try that!

![image](https://github.com/heathbar019/Writeups/assets/114100890/f5c6cf83-aaa4-42b3-872b-f59fe64d3622)

It worked! In the above command, I used the strings command which prints out all readable strings within the file that are at least 4 characters long and I piped into a grep command to search for the flag format "NICC". Doing this yielded the flag "NICC{leaky_data_huh}".

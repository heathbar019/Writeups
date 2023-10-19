# TCP1P CTF - Ez PDF (forensics, 100p, 137 solves) 10/19/2023

> I just downloaded this PDF file from a strange site on the internet....

We are provided with two downloads, one with a .ad1 extension and another with a .ad2 extension. Initially I assumed we were dealing with two different formats for logical image files and that the files were images of the same drive that were both provided just to give more options to use. Later, while trying to replicate this solve I found out that I was wrong, but I'll talk about that more later. For now I'll only investigate the .ad1 file and if I reach a dead end then I'll guess my assumption was wrong and start investigating the .ad2 file. After a quick google search I find out that .ad1 files can be opened with FTK imager, so I'll start there.

![image](https://github.com/heathbar019/Writeups/assets/114100890/80aca858-4294-411a-af2d-5a09983e0986)

With FTK Imager open we can access the .ad1 file by going to File > Add Evidence Item, then selecting Logical Image, and inputting the path to the file. After doing so it should open the logical image file and look like the picture above. Now I'm ready to look around inside this user's computer, but next I need to know where to look. A google search led me to [this](https://support-valorant.riotgames.com/hc/en-us/articles/360048981973-Gathering-Logs-in-VALORANT) Riot Games article on how and where to gather Valorant logs.

![image](https://github.com/heathbar019/Writeups/assets/114100890/04e6e3ea-dfbb-4324-855b-c5204a4597a7)

According to the article, Vanguard Logs can be gathered at the following folder: "C:\Program Files\Riot Vanguard". After locating these logs we can export the whole folder out of ftk imager by right clicking on it and then clicking export files. Now that we have the logs we can start manipulating them to find our flag. Let's print out the contents of one log to see what we're dealing with.

![image](https://github.com/heathbar019/Writeups/assets/114100890/fcfcecbc-5091-44cd-80b7-3d6991c06bde)

As to be expected of logs that are meant to be kept private to Riot, they are encrypted on our local systems. A google search for "riot vanguard log decryptor" will yield the following [forum for hacking games](https://www.unknowncheats.me/forum/anti-cheat-bypass/488665-vanguard-log-decryptor.html).

![image](https://github.com/heathbar019/Writeups/assets/114100890/ccb9d3b1-355e-4e39-9cb3-3eed788049e9)

While I don't want to start hacking games anytime soon, this script will definitely prove useful for our situation. I tested the script on just one of the logs and was a bit surprised by the output.

![image](https://github.com/heathbar019/Writeups/assets/114100890/118ad74c-bb2a-41bc-9cd4-05270726d15f)

At this point I was a bit lost and started overthinking, but with some help and guidance I decided to just try and see if the flag was somewhere in one of these many log files. So I asked ChatGPT (because I'm still working on my scripting skills) to write me a script that will execute a desired command on every file within a specified directory. Then I made some slight tweaks to have it working as I wanted and ended up with the following script.

```
import os
import subprocess

# Define the command to run
command_template = "python vangaurddecryptor.py {}"

# Specify the folder containing the files
folder_path = r"C:\Users\heath\Downloads\Logs"

# Iterate through the files in the folder
for filename in os.listdir(folder_path):
    # Check if the file is a regular file (not a directory)
    if os.path.isfile(os.path.join(folder_path, filename)):
        # Generate the full command by substituting the filename
        full_command = command_template.format(os.path.join(folder_path, filename))
        
        # Execute the command using subprocess
        try:
            subprocess.run(full_command, shell=True, check=True)
            print(f"Command executed successfully for {filename}")
        except subprocess.CalledProcessError as e:
            print(f"Error executing command for {filename}: {e}")
```

Finally, I piped the output of the script to a text file and searched for "vsctf" to get the flag: vsctf{0h_w0W!_v4Ngu4rd_l0Gs_d3CrYpt3D_sHh!!_d0Nt_T3Ll_3vErd0X_>:(}

![image](https://github.com/heathbar019/Writeups/assets/114100890/9ee3ba17-f1be-4402-8e73-250b8dbbe43c)

## Additional notes and research

When trying to replicate the solve for this writeup, I ran into a problem where I wasn't able to open the .ad1 file with FTK imager anymore. As it turns out, my assumption was wrong and its actually necessary to have both the .ad1 and .ad2 file in the same directory for it to work. You can also open either of the files with FTK imager and be able to interact with the same disk image no matter which one you open. Information on this file format online is scarce, but from what I've been able to gather I learned that it is a proprietary disk image format created by AccessData and can only be directly opened by FTK Imager. Because of this its not very popular or well supported. The two files that we download from the ctf are individual files within a set of files that make up the disk image, and therefore are complimentary to each other and need to be together in a directory to function.

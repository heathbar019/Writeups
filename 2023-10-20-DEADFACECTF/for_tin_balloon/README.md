# DEADFACE CTF - Tin Balloon (forensics, 150p, 229 solves) 10/25/2023

> We've discovered that DEADFACE was somehow able to extract a fair amount of data from Techno Global Research Industries. We are still working out the details, but we believe they crafted custom malware to gain access to one of TGRI's systems. We intercepted a Word document that we believe mentions the name of the malware, in addition to an audio file that was part of the same conversation. We're not sure what the link is between the two files, but I'm sure you can figure it out!

This challenge provides us with a zip file that contains a Word document and a mp3 file. We're told that the Word document has the name of the malware inside of it that we need to make the flag, however when I tried to open the document I found out that it is password protected.

![image](https://github.com/heathbar019/Writeups/assets/114100890/b2b8fdbb-4607-4ff7-8cae-589fefeda3b8)

Since I couldn't go any further with the Word doc for the time, I turned my attention to the mp3 audio file. Upon an initial quick listen, it seems to be a normal recording of Stairway to Heaven by Led Zeppelin. However, in the middle of the recording, there are some strange-sounding audio blips for a couple of seconds. I decided to open the file in Audacity to get a closer look.

![image](https://github.com/heathbar019/Writeups/assets/114100890/50d7c190-3c5b-492a-9410-8f6d00049cca)

Above is what the file looks like visualized in audacity, and it's very easy to see the audio spike in the middle will be our point of interest. Zooming into this particular section will help us focus on the important part.

![image](https://github.com/heathbar019/Writeups/assets/114100890/f5aa24ff-9b2f-4fe8-affb-77cdf0e4bc66)

Now, we can try switching to the spectogram view and check for any hidden messages in the audio. We do this by going to the left side of the application where we see sequence settings, clicking the black triangle, and clicking "Spectogram". Doing so reveals the following message:

![image](https://github.com/heathbar019/Writeups/assets/114100890/8517196d-befd-4e4e-9d7a-66c23a6e7dd1)

"Gr33dK1Lzz@11Wh0Per5u3" should be the password to our Word file, and trying it will show us the following conversation:

![image](https://github.com/heathbar019/Writeups/assets/114100890/13b6be5a-e4b9-4101-a3e0-59a167a3e6ab)

From this we discovered the name of the malware that will be used, and can submit the flag; "flag{Wh1t3_N01Z3}".

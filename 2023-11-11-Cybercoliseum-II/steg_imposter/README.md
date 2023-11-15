# Cybercoliseum II - Imposter (steganography) 11/14/2023

![Imposter](https://github.com/heathbar019/Writeups/assets/114100890/7af7fb22-158d-4032-a747-4e2ccc84f049)

In this challenge we are only provided with this picture of an among us crewmate PNG file. Since this is a steganography challenge, it is good practice to run the commands file and exiftool to get some information on the kind of file were working on to see if there is any interesting metadata that can lead us in the right direction.

![image](https://github.com/heathbar019/Writeups/assets/114100890/b8147a0d-7646-4741-a5be-51f773b8964f)

Nothing special here but this does at least confirm that we are working with an actual png file. Now let's try to use binwalk, a tool for extracting hidden files and data.

![image](https://github.com/heathbar019/Writeups/assets/114100890/0f8c2885-bc4e-49a5-a081-56b8c6232708)

This also doesn't yield any interesting results. After trying a couple of different things I tried zsteg, which is a tool for extracting hidden data in PNG and BMP file types by trying various different types of LSB steganography.

![image](https://github.com/heathbar019/Writeups/assets/114100890/288fd36f-d33f-43fc-b1af-540313839df4)

Finally! The second line of output seems to have finally revealed something useful; a url for a image/file sharing website (https://ibb.co/d2TxBjh). When visiting the url we are greated with yet another image.

![sticker](https://github.com/heathbar019/Writeups/assets/114100890/4484a4d2-d0d4-4194-b030-a9925bdb94e2)

Now we are dealing with a JPG. I have no idea what this says but it's probably (hopefully) not important. After running file and exiftool again nothing of importance is found, but since we now have a JPG, it's possible that steghide was used to hide data within our file. We can use a tool called stegseek which is essentially just a cracker for files with data that has been hidden using steghide.

![image](https://github.com/heathbar019/Writeups/assets/114100890/7038b26d-79d6-4b17-a674-425ffa652f89)

Stegseek was successful in finding the correct passphrase "696969" and applying that to the image with steghide allowed it to extract the file "flag.txt". Unsurprisingly, opening this text file will yield us the flag, "CODEBY{an0th3r_str4ng3_st3g4n0gr4phy_h4s_b3en_d3fe4ted}".

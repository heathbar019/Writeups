# TCP1P CTF - Ez PDF (forensics, 100p, 137 solves) 10/19/2023

> I just downloaded this PDF file from a strange site on the internet....

![image](https://github.com/heathbar019/Writeups/assets/114100890/8bcd2697-b2fa-4192-b3d1-ff6afb77a031)

This forensics challenge presents with a lone PDF file, as pictured above. First, I run "file" to check if the file type can be determined as anything other than a pdf, which it is not. Then, I run "exiftool" to read the metadata on the pdf file and look for any fields that might stand out. At the very bottom there is a field labeled "Creator" that has some interesting data in it.

![image](https://github.com/heathbar019/Writeups/assets/114100890/b3933c04-3dd3-438a-b326-374311176df1)

I thought that this string looked like base64 so I pasted it into CyberChef to see if I could get anything.

![image](https://github.com/heathbar019/Writeups/assets/114100890/300ff315-04c8-456a-bb0e-2c3d45394a93)

It was indeed base64! The output string lets us know that the flag has been split into 3 parts and we get given the first part; "TCP1P{D01n9_F023n51C5". Since we are only given this PDF file, the rest of the flag has to be somewhere inside and we just need to look in other places. At some point while I was trying to work this challenge out, someone else who I had been working on this challenge with informed me that another piece of the flag can be found by converting the pdf file to html. This can be done by using the "pdftohtml" command like so:

![image](https://github.com/heathbar019/Writeups/assets/114100890/5f6e89f2-7bbd-4dcc-9f27-832f44ab62d1)

While this looks like it failed, it actually dumped many new files into my working directory as a result of running this command. Opening and inspecting these files will show that there is actually a hidden png file within the pdf that contained the middle part of our flag! I'd also like to note that I was not able to obtain this png file by running "binwalk" on the pdf file and was only able to find it through this method, however I'm sure this can be replicated with other pdf to html tools.

![image](https://github.com/heathbar019/Writeups/assets/114100890/636de2d6-24c9-46eb-9aa5-5d3a9db03ffc)

Now there is only one piece of the flag missing! One of a few methods I tried to find this final flag piece was to look through the hex of the pdf file using GHex hex editor. Towards the beginning of the file I noticed a piece of JavaScript embedded within the pdf.

![image](https://github.com/heathbar019/Writeups/assets/114100890/1387f3a8-694b-44fb-a29d-f9b232b0b294)

I pasted all of the code into an online JS compiler and noticed that this code was very obfuscated and difficult to understand.

![image](https://github.com/heathbar019/Writeups/assets/114100890/ab47be14-9c03-411a-85a9-e963cb7f8aba)

However, I did notice two things; first, I noticed that the whole block of code is contained within an if-else statement and that the variable "whutisthis" can be changed to choose which part of the if-else statement is executed. Second, I noticed that the code within the if portion of the statement is specific to the pdf file and will cause an error when executed by itself. Knowing these two things I changed the "whutisthis" variable to 2 so I could see what the else portion of the code does. After running this modified code, it outputs the final part of the flag!

![image](https://github.com/heathbar019/Writeups/assets/114100890/7beb4c0b-4b84-45c4-b383-4613f073c006)

With this we have all three parts of our flag and can put them together to get "TCP1P{D01n9_F023n51C5_0N_pdf_f1L35_15_345y_15N7_17_l3jaf9ci293m1d}"

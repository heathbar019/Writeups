# TCP1P CTF - Ez PDF (forensics, 100p, 137 solves) 10/19/2023

> I just downloaded this PDF file from a strange site on the internet....

![image](https://github.com/heathbar019/Writeups/assets/114100890/8bcd2697-b2fa-4192-b3d1-ff6afb77a031)

This forensics challenge presents with a lone PDF file, as pictured above. First I run "file" to check if the file type can be determined as anything other than a pdf, which it is not. Then I run "exiftool" to read the metadata on the pdf file and look for any fields that might stand out. At the very bottom there is a field labeled "Creator" that has interesting data in it.

![image](https://github.com/heathbar019/Writeups/assets/114100890/b3933c04-3dd3-438a-b326-374311176df1)


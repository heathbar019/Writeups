# DEADFACE CTF - Git Rekt (traffic analysis, 50p, 364 solves) 10/25/2023

> One of our teammates at Turbo Tactical ran a phishing campaign on spookyboi and thinks spookyboi may have submitted credentials. We need you to take a look at the PCAP and see if you can find the credentials.
> Submit the password as the flag: flag{password}.

![image](https://github.com/heathbar019/Writeups/assets/114100890/8bcd2697-b2fa-4192-b3d1-ff6afb77a031)

This forensics challenge presents with a lone PDF file, as pictured above. First, I run "file" to check if the file type can be determined as anything other than a PDF, which it is not. Then, I run "exiftool" to read the metadata on the pdf file and look for any fields that might stand out. At the very bottom, there is a field labeled "Creator" that has some interesting data in it.

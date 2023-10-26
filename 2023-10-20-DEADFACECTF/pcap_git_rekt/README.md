# DEADFACE CTF - Git Rekt (traffic analysis, 50p, 364 solves) 10/25/2023

> One of our teammates at Turbo Tactical ran a phishing campaign on spookyboi and thinks spookyboi may have submitted credentials. We need you to take a look at the PCAP and see if you can find the credentials.
> Submit the password as the flag: flag{password}.

![image](https://github.com/heathbar019/Writeups/assets/114100890/807d1035-0dc8-4cb0-b2b4-ac6a9834a897)

In this challenge we are given a PCAP file, and are told that we need to find the captured credentials of "spookyboi" from a phishing campaign within the file. We can open up PCAP files in a program called wireshark, which is used to capture and analyze packets. Once open in wireshark, we can see in the bottom left that this file contains a total 3562 packets. We aren't going to look through all of these packets manually so instead we can perform a search. To search for a string, we can go to Edit > Find Packet, then configure the search to "Packet Details" and "String". Now we just need to type in a string! When I solved this challenge I searched for "password" but since we were give the username "spookyboi" you could also search for that too. Either way we will find the following packet.

![image](https://github.com/heathbar019/Writeups/assets/114100890/6ba3027c-9cd5-4a19-8658-b22f737eb283)

Within this packet is an HTML form with the items "login" and "password" from which we can derive the credentials "spookyboi@deadface.io" and "SpectralSecrets#2023".

![image](https://github.com/heathbar019/Writeups/assets/114100890/d04b250f-6947-479f-8042-265e4c224e59)

Since the challenge asks for the password as the flag, then we can submit "flag{SpectralSecrets#2023}"

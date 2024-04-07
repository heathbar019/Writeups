# UTCTF - Fruit Deals (reverse engineering, 380p, 239 solves) 4/6/2024

> I found a excel sheet with some great deals thanks to some random guy on the internet! Who doesn't trust random people off the internet, especially from email
> The flag is the file name that was attempted to be downloaded, wrapped in utflag{} Note: the file imitates malicious behavior. its not malicious, but it will be flagged by AV. you probably shouldn't just run it though.
> By Samintell (@samintell on discord)



![image](https://github.com/heathbar019/Writeups/assets/114100890/807d1035-0dc8-4cb0-b2b4-ac6a9834a897)

In this challenge, we are given a PCAP file and are told that we need to find the captured credentials of "spookyboi" from a phishing campaign within the file. We can open up PCAP files in a program called Wireshark, which is used to capture and analyze packets. Once open in Wireshark, we can see in the bottom left that this file contains a total of 3562 packets. We aren't going to look through all of these packets manually so instead we can perform a search. To search for a string, we can go to Edit > Find Packet, then configure the search to "Packet Details" and "String". Now we just need to type in a string! When I solved this challenge I searched for "password" but since we were given the username "spookyboi" you could also search for that too. Either way, we will find the following packet.

![image](https://github.com/heathbar019/Writeups/assets/114100890/6ba3027c-9cd5-4a19-8658-b22f737eb283)

Within this packet is an HTML form with the items "login" and "password" from which we can derive the credentials "spookyboi@deadface.io" and "SpectralSecrets#2023".

![image](https://github.com/heathbar019/Writeups/assets/114100890/d04b250f-6947-479f-8042-265e4c224e59)

Since the challenge asks for the password as the flag, then we can submit "flag{SpectralSecrets#2023}"

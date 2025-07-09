# b01lers CTF - Chest Mix (miscellaneous, 405p, 60 solves) 4/18/2024

> R3GIRL opened the resulting text, for the first time, a human being read a message from the r3kapig world. There was a warning repeated three times:
> 
> "Do not answer! Do not answer!! Do not answer!!!"
> 
> The message revealed a huge secret, and the fate of the entire human race was now tied to her fingers

## Preface
This CTF was pretty challenging for me and I actually didn't even finish this particular challenge :/ but I got very close! So I wanted to make a writeup anyways since I thought this challenge was interesting, albeit pretty short.

## Solving
We are provided with only a packet capture file called "pig.pcapng", so let's open wireshark and see what we're dealing with. The first thing I like to look at when viewing a packet capture for the first time in wireshark is go to Statistics, then Protocol Hierarchy.

![image](https://github.com/user-attachments/assets/d393cb5d-cffc-44d7-af46-aa86e083fce2)

This will show us what kind of protocols and traffic we actually have in this file. Highlighted in blue, you can see that 97.6% of the file is TCP traffic. The rest of the traffic includes a small amount of MDNS, NTP, ARP, and ping packets which after looking into a little bit, I deemed to be irrelevant to the challenge. So lets right click one of our TCP packets, follow the stream, and see what kind of data is being transferred.

![image](https://github.com/user-attachments/assets/638ca271-95d6-4fac-afd0-399003262cde)

It may momentarily look like random numbers until you notice the beginning, "3.1415926...". Is that pi?? Yes, it's pi. I thought maybe there were some extra numbers or hidden characters mixed in but I threw it in to a string comparing tool and it's really just plain old pi. About 500 numbers worth. And if you look closer you'll notice that it repeats 3 times. But that doesn't really matter because there isn't going to be a way to derive a flag from just these number values on their own, since the numbers of pi are already predetermined and in this case have not been altered in any way. It was time to search elsewhere...


![Untitled video - Made with Clipchamp](https://github.com/user-attachments/assets/974d7bc4-4850-4b9d-afd5-9766e7c88684)

... and a lot of searching I did (many hours worth regrettably), through seemingly normal network traffic until I finally kinda noticed something.

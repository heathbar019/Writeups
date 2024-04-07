# UTCTF - Fruit Deals (reverse engineering, 380p, 239 solves) 4/6/2024

> I found a excel sheet with some great deals thanks to some random guy on the internet! Who doesn't trust random people off the internet, especially from email.
> 
> The flag is the file name that was attempted to be downloaded, wrapped in utflag{} Note: the file imitates malicious behavior. its not malicious, but it will be flagged by AV. you probably shouldn't just run it though.
> 
> By Samintell (@samintell on discord)

## Preface
This CTF challenge involves reverse engineering a Microsoft Excel file and the macro script within it. This excel file comes with pre-enabled macros that, like the challenge introduction says, mimicks malicious behavior. Anti-virus will flag it but do not worry.

![image](https://github.com/heathbar019/Writeups/assets/114100890/e33ca04f-b5ad-4426-8e36-409915697e11)

We are provided with a single excel file called "deals.xlsm", which we are warned has tried to download a malicious file. A common way that threat actors attempt to spread malware onto their victims computers are through a special kind of trojan that utilizes the macro feature on microsoft office file formats like word, excel, powerpoint, etc. These macros can attempt to execute commands on victims computers without them noticing, and this file serves as a good example of this.

# UTCTF - Fruit Deals (reverse engineering, 380p, 239 solves) 4/6/2024

> I found a excel sheet with some great deals thanks to some random guy on the internet! Who doesn't trust random people off the internet, especially from email.
> 
> The flag is the file name that was attempted to be downloaded, wrapped in utflag{} Note: the file imitates malicious behavior. its not malicious, but it will be flagged by AV. you probably shouldn't just run it though.
> 
> By Samintell (@samintell on discord)

## Preface
This CTF challenge involves reverse engineering a Microsoft Excel file. This Excel file comes with pre-enabled macros that, like the challenge introduction says, mimicks malicious behavior. Anti-virus will most likely flag it but do not worry, it is only meant to mimick a malicious file and no harm will come to your system!

![image](https://github.com/heathbar019/Writeups/assets/114100890/e33ca04f-b5ad-4426-8e36-409915697e11)

We are provided with a single excel file called "deals.xlsm", which we are warned has tried to download a malicious file and the name of that file will be our flag. The file extension ".xlsm" is indicative of an Excel macro-enabled workbook file. A common way that threat actors attempt to spread malware onto their victims computers are through a special kind of trojan that utilizes the macro feature on Microsoft Office file formats like word, excel, powerpoint, etc. These macros may attempt to execute commands on victim's computers without them noticing, and this file (as we will soon find out) serves as a good example of this. To get a closer look I'm going to open the file with LibreOffice's Calc on Linux. All of the following steps can be done exactly the same using Microsoft Office's Excel, however it may require some extra steps to trust the file and view the macro.

![image](https://github.com/heathbar019/Writeups/assets/114100890/10ed4b98-a619-41fb-b47c-6b6b733690ea)

Unfortunately, there are no fruit deals anywhere to be found and within the table we are urged to "Enable Content" to see these deals. While this may fool some people, we know better! Newer versions of Office/Libre will automatically disable any macros within files, and this instruction is the threat actor's attempt at getting the user to bypass this safety restriction themselves. If you look closely at the yellow banner at the top of the screen, Libra allows us to view the macros without enabling them if we click "Show Macros".

![image](https://github.com/heathbar019/Writeups/assets/114100890/d7dfda30-d3fe-49d0-89f1-d58ebdaa52df)

Within the Macro Organizer we can see two macros called "Module1" and "Module2". Let's take a look at Module1 first.

![image](https://github.com/heathbar019/Writeups/assets/114100890/a8dd1c05-20c9-4445-bfc6-303b1aed9e6f)

Quickly analyzing this code will tell you that this macro is working in "Sheet2", iterating through evey cell within the range A1 to AA100, and filling it with 8 characters of a random base64 string.

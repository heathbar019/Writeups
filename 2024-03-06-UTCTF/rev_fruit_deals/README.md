# UTCTF - Fruit Deals (reverse engineering, 380p, 239 solves) 4/6/2024

> I found a excel sheet with some great deals thanks to some random guy on the internet! Who doesn't trust random people off the internet, especially from email.
> 
> The flag is the file name that was attempted to be downloaded, wrapped in utflag{} Note: the file imitates malicious behavior. its not malicious, but it will be flagged by AV. you probably shouldn't just run it though.
> 
> By Samintell (@samintell on discord)

## Preface
This CTF challenge involves reverse engineering a Microsoft Excel file. This Excel file comes with pre-enabled macros that, as the challenge introduction says, mimic malicious behavior. Anti-virus will most likely flag it but do not worry, it is only meant to mimic a malicious file and no harm will come to your system!

![image](https://github.com/heathbar019/Writeups/assets/114100890/e33ca04f-b5ad-4426-8e36-409915697e11)

We are provided with a single Excel file called "deals.xlsm", which we are warned has tried to download a malicious file and the name of that file will be our flag. The file extension ".xlsm" is indicative of an Excel macro-enabled workbook file. A common way that threat actors attempt to spread malware onto their victim's computers is through a special kind of trojan that utilizes the macro feature on Microsoft Office file formats like Word, Excel, Powerpoint, etc. These macros may attempt to execute commands on victim's computers without them noticing, and this file (as we will soon find out) serves as a good example of this. To get a closer look I'm going to open the file with LibreOffice's Calc on Linux. All of the following steps can be done exactly the same using Microsoft Office's Excel, however, it may require some extra steps to trust the file and view the macro.

![image](https://github.com/heathbar019/Writeups/assets/114100890/d84dc7a0-23da-4558-8cb0-fa06df83fab1)

Unfortunately, there are no fruit deals anywhere to be found and within the table, we are urged to "Enable Content" to see these deals. While this may fool some people, we know better! Newer versions of Office/Libre will automatically disable any macros within files, and this instruction is the threat actor's attempt at getting the user to bypass this safety restriction themselves. If you look closely at the yellow banner at the top of the screen, Libre allows us to view the macros without enabling them if we click "Show Macros".

![image](https://github.com/heathbar019/Writeups/assets/114100890/d7dfda30-d3fe-49d0-89f1-d58ebdaa52df)

Within the Macro Organizer, we can see two macros called "Module1" and "Module2". Let's take a look at "Module1" first.

![image](https://github.com/heathbar019/Writeups/assets/114100890/a8dd1c05-20c9-4445-bfc6-303b1aed9e6f)

Quickly analyzing this code will tell you that this macro is working in "Sheet2", iterating through every cell within the range A1 to AA100, and filling it with 8 characters of a random base64 string. Sheet2? Initially, this may confuse you, as it did me, because if you look back at our worksheet there is only one worksheet called "Deals". After some clicking around and thinking, I remembered that you can hide/show worksheets by clicking on the bar at the bottom that displays the currently open worksheets.

![image](https://github.com/heathbar019/Writeups/assets/114100890/88265e7e-f955-45b3-b01e-f2ed1285969e)
![image](https://github.com/heathbar019/Writeups/assets/114100890/9b389b10-82b2-44c2-b2ca-959faabe0018)

Now we can open up sheet2 and see what's going on...

![image](https://github.com/heathbar019/Writeups/assets/114100890/31f23e50-929b-43fa-97f1-4c2b049acc14)

So it isn't very pretty, but it's basically what we expected, many cells filled with random 8-character base64 strings. This doesn't really mean much to us right now so let's take a look at the second macro, "Module2".

![image](https://github.com/heathbar019/Writeups/assets/114100890/c404ce3b-8914-456b-9cc3-b7960b396cbd)
![image](https://github.com/heathbar019/Writeups/assets/114100890/1b24aae3-977f-4ac1-8eed-a39381e920a9)

This isn't very pretty to look at either. The macro is quite lengthy but fairly simple to summarize. A string "f" is declared, followed by six more strings with long alphanumeric names. Then, through a series of a little under 200 lines worth of if-then statements, substrings are appended to each of the six oddly-named strings by comparing values from the cells of "sheet2" to base64 strings. After that, it concatenates the six strings into the string "f" and attempts to execute the result as a command. It also attempts to open up a youtube video of a cantaloupe but this is unimportant. The path from here is fairly straightforward, we need to find f by concatenating each of the strings. Some other writeups that I reviewed for this challenge had an efficient approach to finding f by modifying the macro to simply print f into a cell after doing all the work instead of executing it. I, unfortunately, did not have this intuition while solving the challenge and instead opted for concatenating the strings manually... In the end, after too much time, I concatenated the following string for f.

![image](https://github.com/heathbar019/Writeups/assets/114100890/c2064735-d752-4414-9728-e54ea714904a)

This command can be broken down into a few key parts:
* First, it launches a Powershell session and specifies that it wants to execute a command.
* Then, it creates a .NET WebClient object and assigns it to "$oaK". This class is commonly used for downloading internet files via PowerShell.
* Next, the variable "$0rA" is set to the URL 'http://fruit\.gang/malware'. This is where the malware will be downloaded from.
* Then, the variable "$CNTA" is set to the string 'banANA-Hakrz09182afd4', which is the name of the file being downloaded. This is our flag!
* Next, it creates a file path that indicates where the malware will be downloaded and stores it in "$jri".
* Finally, within a try-catch block, it attempts to download the file from the previously mentioned URL and attempts to execute it using the "Invoke-Item" cmdlet.

After analyzing this command, we know that the malicious file that was attempted to be downloaded is called 'banANA-Hakrz09182afd4', and from this, we can create the flag: utflag{banANA-Hakrz09182afd4}

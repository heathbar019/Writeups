# Spooky CTF - Where we're going... (osint, 745p, 12 solves) 11/2/2023

> If we're going to explore the universe like the Zorglax, we're going to need a way to get there. I bet there's some geniuses out there who have already figured it out. I heard someone say last November that there was
> a breakthrough in spacetime propulsion out of Florida of all places. I can't speak for other nations, but I know the US has some real geniuses who have probably already figured it out. I just hope that they patent it so
> that they get the credit they deserve. If you see anything, can you tell me the unique id so I can look it up? Make sure to give it to me exactly as it appears, wrapped in NICC{} of course.

For this osint challenge, we are tasked with finding a particular patent's ID. We are told that this patent has something to do with spacetime propulsion, and was created last November by someone out of Florida. My first instinct here is to search for some kind of patent database, and hopefully a free one. I do a Google search for "patent database search" and the very first result yields a page titled "search for patents" on the United States Patent and Trademark Office. Perfect!

![image](https://github.com/heathbar019/Writeups/assets/114100890/200511fb-d455-4fd9-a0ce-757dc208e695)

There seem to be many different ways to search for patents, so let's just try the first option, "Patent Public Search". We are then given an option between a basic and an advanced search. For now, the basic search should do just fine.

![image](https://github.com/heathbar019/Writeups/assets/114100890/5534ad35-d624-430e-bc69-639f003bb94c)

Now we are on a page where we can finally input our search. There are two text boxes where we can search for individual words, so we can try "spacetime" and "propulsion". This search will yield us a surprising amount of results, however, the patents are sorted by publication date so finding a result from last year should be simple.

![image](https://github.com/heathbar019/Writeups/assets/114100890/9e4be3ba-f1ab-48de-9deb-0294f875b7c0)

Here we can see that the 5th result is a patent published on November 24th, 2022 and it's titled "ELECTROMAGNETIC SPACETIME CONTINUUM PROPULSION SYSTEM FOR SPACE TRAVEL". Looks good! From this, we can create the flag from the patent ID on the left side and we get "NICC{US-20220371752-A1}"

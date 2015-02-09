---
layout: post
title:  Bash Script for Simple Screenshot Utility
date:   2015-02-09 15:00
categories: bash script simple coding screenshot 
---
Hello!

Today's post is less artsy and less projecty. I'm going to be walking through a bash script I wrote. My hope is to be able to explain it to people who are still in the learning stages, much like I am.

I have been (slowly, but surely) making my way through the JavaScript lesson on [Codecademy](http://www.codecademy.com/). There's hints and a link to a Q&A forum, but sometimes I get help from friends through IRC. (Come hang out with us. Go to [hashbang.sh](http://hashbang.sh) and follow the command). When I get help from friends, I can pretty easily give them the code through places like [Pastebin](http://pastebin.com) and [pae.st](http://pae.st), but usually my problem is stemming from unclear instructions (codecademy does well, but sometimes I think being so far away from the beginning there's some miscommunication). Once I've given my friend a link to the code I then either have to paraphrase what the instructions are or copy the instructions to a google doc and send that to them somehow. It just takes too long and can be somewhat frustrating. You might be wondering, "why don't you just 'print screen'?" well, I have the image, but they still don't. I still need to upload it somewhere, send them a way to get there, etc etc. This is where I started brainstorming a solution. 

First thing was to figure out the steps I wanted my future script to do.
 
 * It would need to create a new folder like .screenshots, but only if it wasn't already created (you know, for the 2nd, 3rd, 4th, etc time around).
 
 * I would need a variable for a unique file name. Easiest is probably the date and time, so for example 2015-02-09-16-25-33.png. 

 * It would need to import the image and save it to the .screenshot folder with the generated filename.
 
 * It would need to make a url so that I could share the screenshot and, to make sharing even easier, already have the url in my clipboard so all I'd have to do after selecting my screenshot is paste it where I want. 

Most, if not all, scripts will have variables. Basically just like what you learn in your first algebra class and probably even before then. It's just like 1/2b * h to find the area of a triangle. b meaning the base and h meaning the height. And you just substitute the numbers in. Turns out the majority of this screenshot script is variables. You'll see that at the beginning of my script I have my list of variables. This is so a new person using my script only has to change the specific names of things once instead of every time they show up in the script.  

Here's a [link](https://github.com/stinethebean3/screenshot) to my script on github for you to follow along.

 * Make a folder only if it doesn't already exist. 

After a quick Google search I found that `mkdir -p "VARIABLE"` is all that is needed. [Stack overflow](http://stackoverflow.com/questions/59838/check-if-a-directory-exists-in-a-shell-script) had the answer. `mkdir` is a command used to "make (a) directory" and `-p` "means make parent directories as needed."

After `mkdir -p` you'd put the directory name, but it's a variable, so you'd change it at the top where the list of variables are. Mine is in my home folder under the folder name of .screenshot (a . in front of a file/folder will make it hidden).

{%highlight bash%}
mkdir -p "$SCREENSHOT_DIR"
{%endhighlight%}

I also made a symbolic link for a shortcut command to only have to type `ss` instead of `screenshot` each time I wanted to run the script just by writing this command in the terminal.

	ln -s screenshot.sh ss

 * Import (save) the screenshot to the folder. 

This step is pretty simple. It's just the word `import` and then the variable for your directory/filename. The / represents the inside of the folder. 

{%highlight bash%}
import "$SCREENSHOT_DIR/$SCREENSHOT_FILE"
{%endhighlight%}

The filename will be a variable because each time you take a screenshot the image will need to have a different name else it the 2nd screenshot will overwrite the 1st one.  I went with the date and time down to the seconds because it is possbile you may want to take multiple screenshots in a minute. So the variable becomes

{%highlight bash%}
SCREENSHOT_FILE="$(date +%y-%m-%d-%H-%M-%S).png"
{%endhighlight%}

 * Copy the file to a public directory on a web server for easy sharing. 

The command for that is `scp` which means "secure copy" (to another computer). 

{%highlight bash%}
scp "$SCREENSHOT_DIR/$SCREENSHOT_FILE" "$SSH_USERNAME@$SSH_HOSTNAME:$PUBLIC_FOLDER"
{%endhighlight%}

This is copying the file, which is found in that specific directory, over to your server. I decided to go with my own public webserver over something like [Imgur](imgur.com) because imgur may not always be here (or might change their API a.k.a. how to post to their website). I can however make sure that my own personal server stays up. 

 * Echo the url and send to clipboard

If you were to type `ss` into the command line in a terminal it would output the url there, just as sort of a check and balances that it worked. But you don't have to go into the terminal to run this command. I use the [Awesome window manager](http://awesome.naquadah.org/) and just have to press win-r for run and type `ss` there. Most window managers have a similar shortcut to "quick run" a command. (Alt+F2 does the same thing in Ubuntu for instance).

The `echo "$FULL_URL" | xclip` is there to send the url to your clipboard so you only have to paste the url. 

When you run the command you're cursor will become a crosshair. Just click and pull what you want to screenshot. It doesn't even have to be the whole page. Then it's already created the filename and copied it to your clipboard. Easy-peasy!

Here's a random [screenshot](http://stinethebean.hashbang.sh/15-02-09-15-34-43.png) I took while finalizing this blog post!

Bye!

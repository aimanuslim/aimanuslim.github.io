---
title: Commenting Uncommeting Print Statements in Python Like a Pro with Vim
date: 2020-08-19T03:15:23.802Z
description: Title.
---
You are stuck with a terminal. You could get by with Sublime, or VS Code, but since the internet sucks bananas, connecting  through RDP would probably grow you  armpits hair before you could even send a single letter to screen.

So you went to your secret toolbox and pulled it out. 



![](vim.png)



You were editing some python codes. And you can't opt for pdb because whoever crapped before you left the code in shambles that its too lethargic to even run properly. So you had to put print statements for debugging the crap in there too. Except, that vim does not know you are editing Python (much less how to comment the lines automatically). Heck, it was born 50 centuries ago.



So you polished your toolkit and you say, you know what, I am going to leave all my print statements in here. Since the management is too lazy to even look at your code, you know it wont matter having those babies all over the place. 



But somehow, you need the code to not regurgitate text while its run by the users. I mean we need privacy for computers too you know. So somehow, you had to uncomment all these babies before you push it to production. How could you figure out a way to do such an abominable endeavour?

Fret not.

`:%s/^\(\s.*\)print/\1# print/gc`

Let's dissect that together shall we?

`%s `- thats the command for search and replace over the whole text file.

`^` - means start of the line. Since you dont want to replace print strings that lies somewhere in the middle of sentences for whatever devilish reasons, you had to put this in.

`\(\s.*\)` - well this is wildcard to capture all the whitespaces that exist after the start of the line. This should tackle your condition that you do not want to comment an already commented line like `# print`. Bro, you nag on these babies too many times you gon make em cry man.

`\1` - take all that captured whitespaces and put it back here. In other words, keep the indents to broker world peace.

`g` - do this globally (as if you haven't already know this).

`c` - confirm, just so that you don't mess up again like you always do.
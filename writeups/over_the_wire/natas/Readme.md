---
tags:
  - ctf/otw/natas
  - type/writeup
creation date: 2023-10-04 09:55
modification date: 2023-10-04 09:55
resource:
---
---
# Over the Wire - Natas

```table-of-contents
```

## About
https://overthewire.org/wargames/natas/
The Natas wargame on "Over the Wire" is intended as an introduction to web-security. Especially in the later levels it has a focus on the serverside aspects of this. 
It is very beginner friendly, although it does not provide the same level of support, hints and advice as the Bandit wargame.
### How it works
The Natas wargame consists of 34 levels. Each level is centered around a specific subdomain of `natas.labs.overthewire.org`. Level 0 is `natas0.natas.lab.overthewire.org`, level 16 is `natas16.natas.lab.overthewire.org` and so on. 
There is a dedicated user for each level (natas0, natas1...) and only this user has access to 'their' subdomain. The access is restricted with basic HTTP authentication.
In order to get to the next level you need to *somehow* find the password of the next user. Depending on the level those passwords are placed somewhere on the webpage or (especially in the higher levels) can only be accessed via a file stored on `/etc/natas_webpass/` on the webserver. For example the password for natas16 is stored in `/etc/natas_webpass/natas16`. Those files are always readable by the user itself and the user of the previous level. This means that, if you managed to get access to this folder you can obtain the next password.
## Some Advice

You have never touched a web-challenge before? How the web works is still a little fuzzy for you? 
Don't worry. The Natas wargame is meant for you. 
Here are a few tips in case you don't know where to start
#### Get an overview
Always begin by getting an overview. There are a few things that you should always do for every level where you don't immediately know what to do. 
1. Take a close look at the rendered webpage that your browser depicts. Read the text. Try out subpages. Follow Links. 
2. Look at the  source of the webpage (hint: use your browsers 'inspect' function)
3. If the first two steps didn't provide you with at least a starting point, check out the HTTP Request and Response (including the headers!)
#### Ask questions
This may sound stupid, but asking yourself questions if - in my opinion - **the** best way to solve any challenge. Be it a beginner web-challenge, debugging your own code, doing incident response, etc.
Ask yourself questions like:
- where does this data come from? 
- how does this part work?
- how is information exchanged?
- what is happening in the background?
It may feel weird in the beginning, but practice asking yourself all the questions and then trying to answer them. It really helps
#### Take Notes
Especially on the more complicated levels this is a must! But in order to get used to it and practice, I recommend to start already at the easier levels.
Write down everything that helped you get to the solution, new things you learned, which tools you used, maybe even things you tried that did not work. This has multiple advantages:
- it helps you memorize specific things
- even if your memory is trash like mine, you can look stuff up again
- you could, if you want, easily create a writeup
- you don't loose track on the more time-intensive and complicated challenges

> **HINT:** especially for the over the wire wargames, write down the passwords of all the levels you already did. Since every level is based on the result of the previous level, loosing the password means that you have to start from the very beginning. Believe me you don't want that. Learn from my mistakes
#### Writeups ftw
From time to time, even with the very helpful tips above you may be completely stumped. If that happens, don't worry. That is what writeups are for. 
If you don't know what to do even though you have tried a few things, done the basics mentioned above and spent some time thinking it through, there is no shame in looking up a writeup. In order to still learn as much as possible and not just follow someone elses instruction click-by-click I recommend to read a writeup only as far as necessary to get a new hint/idea/consideration and try again without help from there. If you are stuck again, feel free to go back to the writeup.
## My writeups
This repo includes my writeups for the Natas wargame. I created those writeups as help for students of a lecture I gave (however, if they help others as well then I won't complain :)). They are meant to be beginner friendly and thus may go into more detail than necessary.
The writeups are structured in a way that I hope facilitates 'thinking for yourself' without having people stuck on one level for hours. Every writeup begins with a list of keywords and a list of questions. 
The keywords are starting points for researching concepts that may be needed for this level. The questions are questions to ask yourself when confronted with the challenge (see [[#Ask questions]]).
Those two parts may already be enough of a jump start to get the reader to solve the challenge on their own. If not, the rest of the writeup is a classic step-by-step guide to solving the challenge.
At the end I sometimes added links for further reading. Those can be used if a challenge sparks a deeper interest or if a lot of help was required and you want to dig a little deeper.

> Note: the writeups, as the wargame, are based on the assumption that you start with level 0 and work through them 'chronologically'. Thus, learnings from earlier levels are not repeated again. I tried to refer back to the level that already covered this concept though.
---
title: "Natas1"
date: 2023-11-15T17:06:42Z
draft: false
toc: false
images:
tags: 
  - untagged
---
# Natas 1 -> Natas2

| url: | http://natas1.natas.labs.overthewire.org/|
| --- | -----|

> **Keywords\*:** html, inspect element (browser)
> *\*  if you are stuck with this and don't know where to start or even look, you can research the keywords instead of reading the whole write-up. This will give you the chance to solve the challenge yourself with just a small hint *

> **Questions to ask:**
> Which workarounds for the right-click are there?
## Getting an overview
Similarly to the last level the webpage displays a header and a simple textbox:
![Pasted image 20231003090829.png](/Pasted%20image%2020231003090829.png)
Since the password is supposedly on this page but we can't see it in the rendered website, we can assume that it is hidden somewhere in the sourcecode. 
However, this time the textbox claims that right-clicking is blocked. And indeed, if we rightclick in the textbox instead of our trusty browser menu we get a notification:
![Pasted image 20231003090938.png](/Pasted%20image%2020231003090938.png)

## Digging Deeper
There are two ways to solve this challenge. 
1. We can use the shortcut of our browser of choice to enter the sourcecode. For firefox this is `ctrl` + `shift` + `c`. 
2. We can right-click somewhere on the webpage **outside** of the textbox
Either way we get to the 'inspection' view of our browser and can view the sourcecode
![Pasted image 20231003091651.png](/Pasted%20image%2020231003091651.png)

Similar to Natas0 we can now see the password in a html comment.


---
title: "Natas 0 -> Natas 1"
date: 2023-11-15T17:05:14Z
draft: false
toc: false
images:
tags: 
  - untagged
password: fOIvE0MDtPTgRhqmmvvAOt2EfXR6uQgR
---

| url |http://natas0.natas.labs.overthewire.org/|
| ---| --|
| initial password | natas0 |

> **Keywords\*:** html, inspect element (browser)
> *\*  if you are stuck with this and don't know where to start or even look, you can research the keywords instead of reading the whole write-up. This will give you the chance to solve the challenge yourself with just a small hint *

> **Questions to ask:**
> Which information is not displayed by the browser?

## Getting an overview
On the website we can see a header and a textbox:
![testimage](/natas0_first_screenshot.png)
The textbox claims that the password can be found on this page. However, viewing the rendered page does not show us a password.
So what now?

## Digging Deeper
When looking at a website from a security perspective it makes sense to not only trust the rendered website (what the browser depicts) but also check the sourcecode. This can be done by using the 'Inspect' function of the browser of your choice. For firefox you can either do  `rightclick` + `inspect`  or use the shortcut `strg` + `shift` + `c`.
The sourcecode of this webpage looks like this:
![Pasted image 20231004120340.png](/Pasted%20image%2020231004120340.png)
We can see the html code defining the `div` displaying the text we saw earlier. Additionally we can see a html comment (`<!-->` syntax). This comment contains our next password!

## Further Reading
https://www.w3schools.com/html/html_comments.asp

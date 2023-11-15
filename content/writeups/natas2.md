---
title: "Natas 2 -> Natas 3"
date: 2023-11-15T18:49:01Z
draft: false
toc: false
images:
tags: 
  - untagged
---

| url |http://natas2.natas.labs.overthewire.org/|
| ---| --|

> **Keywords\*:** html file paths 
> *\*  if you are stuck with this and don't know where to start or even look, you can research the keywords instead of reading the whole write-up. This will give you the chance to solve the challenge yourself with just a small hint *

> **Questions to ask:**
> If not on this page, where could the password be?

## Getting an overview
![Pasted image 20231003091850.png](/Pasted%20image%2020231003091850.png)
For level 2 of Natas we see the same website consisting of a header and a single textbox. This time the text inside implies that the password can't be found on this page. Still the first step should always be checking out the sourcecode. Maybe we can find some hints. If you don't know how to view the sourcecode of a website in the browser ask the search engine of your choice.
## Digging deeper  
![Pasted image 20231003082418.png](/Pasted%20image%2020231003082418.png) 
This time, as promised by the textbox, there is no obvious comment containing the password. We'll have to dig a little deeper. 
One thing to notice when reading the sourcecode is the linked image. We can't see the image on the rendered page but we can see that it is there in the sourcecode:
`<img src="files/pixel.png">`
We can also see that it is a link to a file on the same webserver. 
Interesting...
## Following the traces
Maybe we can access the folder that contains the image. The name `files` suggests that there may be other interesting content in there.
And when browsing to `http://natas2.natas.labs.overthewire.org/files/` we can actually see that the folder contains two files
  ![Pasted image 20231003082528.png](/Pasted%20image%2020231003082528.png)
## Accessing `users.txt`
`users.txt` is an interesting file name. I wonder what it contains? Let's take a look.
![Pasted image 20231003082720.png](/Pasted%20image%2020231003082720.png)
And bingo! we can see a list of username:password combinations containing the user `natas3`

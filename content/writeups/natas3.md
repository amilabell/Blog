---
title: "Natas 3 -> Natas 4"
date: 2023-11-16T10:18:12Z
draft: false
toc: false
images:
tags: 
  - untagged
---

| url |http://natas3.natas.labs.overthewire.org/|
| ---| --|

> **Keywords\*:** directory brute-forcing, Robots Exclusion Protocol
> *\*  if you are stuck with this and don't know where to start or even look, you can research the keywords instead of reading the whole write-up. This will give you the chance to solve the challenge yourself with just a small hint *


> **Questions to ask:**
> How do we find subpages of a website?

## Getting an overview
As always let's start with the two preliminary steps: viewing and reading the rendered webpage and viewing and reading the sourcecode.
The website is the same header and textbox combo of the Natas 2.
![Pasted image 20231003092857.png](/Pasted%20image%2020231003092857.png)
The sourcecode contains only the textbox and a html comment informing us that there are no information leaks this time. This references back to how we found the password in the last level using a filepath to an image as 'information leak'.
![Pasted image 20231003083155.png](/Pasted%20image%2020231003083155.png)

Unfortunately it looks like we have no such luck this time :(

## Bruteforce all the directories
Now that it seems that the easy way is not possible lets look a little closer. Maybe the password is stored in a file somewhere on the server like last time. But how do we find this without any hint in the source? 
This technique called 'directory brute-forcing' or 'directory enumeration' can be help. By using a wordlist of commonly found/used filenames on a webserver and iterating over those appended to your source URL you may be able to get lucky. Of course you can script that yourself but there are also readily made command-line tools out there. One of the most popular ones is `dirbuster`.

We will use the following command:
![Pasted image 20231003095409.png](/Pasted%20image%2020231003095409.png)
1. `dirb` to call dirbuster
2. the base-url that we wan't to brute-force subpages on
3. the http username and password combination
This will yield us the following results:

```
-----------------
DIRB v2.22    
By The Dark Raver
-----------------

START_TIME: Tue Oct  3 09:46:01 2023
URL_BASE: http://natas3.natas.labs.overthewire.org/
WORDLIST_FILES: /nix/store/0ra6as4q4j8521454g94wvg1dxckrhfy-dirb-2.22/share/dirb/wordlists/common.txt
AUTHORIZATION: natas3:<removed_password>

-----------------

GENERATED WORDS: 4612                                                          

---- Scanning URL: http://natas3.natas.labs.overthewire.org/ ----
+ http://natas3.natas.labs.overthewire.org/cgi-bin/ (CODE:403|SIZE:298)                            
+ http://natas3.natas.labs.overthewire.org/index.html (CODE:200|SIZE:923)                          
--> Test                                                                                            + http://natas3.natas.labs.overthewire.org/robots.txt (CODE:200|SIZE:33)                           
+ http://natas3.natas.labs.overthewire.org/server-status (CODE:403|SIZE:298)                       
                                                                                                   
-----------------
END_TIME: Tue Oct  3 09:48:44 2023
DOWNLOADED: 4612 - FOUND: 4

```

We can see that based on the default wordlist four files/directories were found. The most interesting one is definitely `robots.txt`.

> **Hint** the robots.txt file is a standard established to tel web crawlers which pages they are allowed to visit. 

## Checking out robots.txt
![Pasted image 20231003100056.png](/Pasted%20image%2020231003100056.png)
When accessing `http://natas3.natas.labs.overthewire.org/robots.txt` we can see that for all user-agents the subpage `/s3cr3t/` is excluded. This is probably what was meant by the html comment claiming that not even Google will find the information leak this time. Because Googles web-crawlers adhere to the Robots Exclusion Protocol and thus follow the instructions mentioned in the robots.txt.

## Accessing the secret webpage
Following the breadcrumbs the next logical thing to do is accessing the path with the promising name: `http://natas3.natas.labs.overthewire.org/s3cr3t/`

![Pasted image 20231003100450.png](/Pasted%20image%2020231003100450.png)
Similar to level 2 we found a directory containing a `users.txt` file.

## Checking out users.txt
Accessing `http://natas3.natas.labs.overthewire.org/s3cr3t/users.txt` provides us with our next username:password combo
![Pasted image 20231003100700.png](/Pasted%20image%2020231003100700.png)


## Some notes

Obviously for someone with a background in either web-development, web penetration testing, etc. checking the `robots.txt` file seems natural and can be done without brute-forcing directories first. However this writeup is intended for beginners that may look at a webpage like this and not know about any of the 'standard ways'.

## Further reading
https://developers.google.com/search/docs/crawling-indexing/robots/intro
https://medium.com/@hninja049/directory-enumeration-1ba91012dcf8

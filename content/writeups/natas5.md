---
title: "Natas5"
date: 2023-11-15T18:49:11Z
draft: false
toc: false
images:
tags: 
  - untagged
---
# Natas 5 -> Natas 6

| url | http://natas5.natas.labs.overthewire.org/ | 
|---| -- |

> **Keywords\*:** cookies, web proxy
> *\*  if you are stuck with this and don't know where to start or even look, you can research the keywords instead of reading the whole write-up. This will give you the chance to solve the challenge yourself with just a small hint *

 **Questions to ask:**
> Which technology is use to check if a user is logged in?
> How can I manipulate the logged-in state?

## Getting an overview
As always we start by taking a look at the webpage.
![x](/Pasted%20image%2020231003110953.png)
Apparently we have to be logged in to view the content (presumably the next password). Since we successfully authenticated with the credentials of natas5, there seems to be another login-check implemented. What could that be? 

## Digging deeper
In [Natas 4](/Natas%204.md) we learned about burpsuite and that intercepted HTTP traffic can be manipulated before it is forwarded again. So let's see if this also helps in this level.
Additionally to viewing the rendered and source version of the webpage (as we did in the earlier levels) let's also take a look at the requests made. 
We start by loading the page in the burp browser once with interception turned off, providing username and password. 
Then we turn interception on and reload the page.
We get the following HTTP request:
```
GET /index.php HTTP/1.1
Host: natas5.natas.labs.overthewire.org
Cache-Control: max-age=0
Authorization: Basic bmF0YXM1OlowTnNydElrSm9LQUxCQ0xpNWVxRmZjUk44MkF1Mm9E
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36
(KHTML, like Gecko) Chrome/114.0.5735.110 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,
image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9
Cookie: loggedin=0
Connection: close
```

Since the website informed us that we can't access the page because we are not logged in, the `Cookie: loggedin=0` line immediately jumps out.

> **Note** this can of course be checked without burpsuite by using the browsers native inspection (see image). However you can't manipulate the settings directly in the browser without a 'mitm proxy'

![Pasted image 20231003111455.png](/Pasted%20image%2020231003111455.png)

### ~~Eating~~ Changing the Cookies
Luckily, it could not be easier to change the cookie value to 1 in our intercepted request in burpsuite:
![Pasted image 20231003111640.png](/Pasted%20image%2020231003111640.png)

## Profit
Now forwarding the manipulated request brings us to this page:
![Pasted image 20231003111745.png](/Pasted%20image%2020231003111745.png)

## Further Reading
https://http.dev/cookies
https://portswigger.net/support/using-burp-to-hack-cookies-and-manipulate-sessions⏎   

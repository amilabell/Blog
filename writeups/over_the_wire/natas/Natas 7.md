---
tags:
  - ctf/otw/natas/7
  - type/writeup
creation date: 2023-10-03 12:01
modification date: 2023-10-03 12:01
resource: 
password: jmxSiH3SP6Sonf8dv66ng8v1cIEdjXWr
---
---
# Natas 7 -> 8

| url | http://natas7.natas.labs.overthewire.org/ | 
|---| -- |

> **Keywords\*:** url parameter
> *\*  if you are stuck with this and don't know where to start or even look, you can research the keywords instead of reading the whole write-up. This will give you the chance to solve the challenge yourself with just a small hint *

 **Questions to ask:**
> what happens with the user input on the serverside?
> how can we use the input to trigger unwanted behaviour on the server?

## Getting an overview
Same as the previous 6 levels. Let's start by taking a look at the rendered webpage and its source:
![Pasted image 20231003121639.png](Pasted%20image%2020231003121639.png)
![Pasted image 20231003121712.png](Pasted%20image%2020231003121712.png)

On the first glance we see two links in a box. In the source we can see those links linking to 'index.php?page=home' and 'index.php?page=about'. We also see a HTML comment informing us that the location of the password in '/etc/natas_webpass/natas8'.
What now?
Let's play around a little and click on one of those links.
![Pasted image 20231003121937.png](Pasted%20image%2020231003121937.png)
We see that the URL has changed to 'http://natas7.natas.labs.overthewire.org/index.php?page=home'. We also see that the website now displays an additional string.

> **HINT:** ?page=home is a so called URL parameter. '?' marks the start of a parameter, 'page' is the name and 'home' is the value.
i

## Digging deeper
Seeing that a URL parameter called 'page' is used and currently set to 'home' implies that the webserver takes this parameter and serves the file with the given value as a name. 
Let's test this theory a little.
What happens when we click on 'About':
![Pasted image 20231003122341.png](Pasted%20image%2020231003122341.png)
What happens when we put in some random value instead of 'about':
![Pasted image 20231003122443.png](Pasted%20image%2020231003122443.png)
This suggests that our initial hypothesis is correct. 
## Combining the pieces:
We know two things:
1. that the webserver attempts to serve the value of the URL parameter 'page'
2. that the password of natas8 is stored in '/etc/natas_webpass/natas8'

So what happens if we add '/etc/natas_webpass/natas8' as a value for 'page'?
![Pasted image 20231003122816.png](Pasted%20image%2020231003122816.png)

## Further Reading:
https://www.semrush.com/blog/url-parameters/
https://owasp.org/www-community/attacks/Web_Parameter_Tampering
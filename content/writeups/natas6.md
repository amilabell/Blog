---
title: "Natas 6 -> Natas 7"
date: 2023-11-15T18:49:14Z
draft: false
toc: false
images:
tags: 
  - untagged
---
---

| url | http://natas6.natas.labs.overthewire.org/ | 
|---| -- |

> **Keywords\*:** php
> *\*  if you are stuck with this and don't know where to start or even look, you can research the keywords instead of reading the whole write-up. This will give you the chance to solve the challenge yourself with just a small hint *

 **Questions to ask:**
> How is the secret evaluated?

## Getting an overview
As always, we will start by looking at the webpage:
![Pasted image 20231003113713.png](/Pasted%20image%2020231003113713.png)
There are two things that stand out:
1. it seems that this time we have to input a secret
2. we get a link to the sourcecode
## Digging deeper
Before trying to blindly bruteforce the secret, let's take a look at the sourcecode:
```html
<html>  
<head>  
<!-- This stuff in the header has nothing to do with the level -->  
<link rel="stylesheet" type="text/css" href="http://natas.labs.overthewire.org/css/level.css">  
<link rel="stylesheet" href="http://natas.labs.overthewire.org/css/jquery-ui.css" />  
<link rel="stylesheet" href="http://natas.labs.overthewire.org/css/wechall.css" />  
<script src="http://natas.labs.overthewire.org/js/jquery-1.9.1.js"></script>  
<script src="http://natas.labs.overthewire.org/js/jquery-ui.js"></script>  
<script src=http://natas.labs.overthewire.org/js/wechall-data.js></script><script src="http://natas.labs.overthewire.org/js/wechall.js"></script>  
<script>var wechallinfo = { "level": "natas6", "pass": "<censored>" };</script></head>  
<body>  
<h1>natas6</h1>  
<div id="content">  
  
<?  
  
include "includes/secret.inc";  
  
    if(array_key_exists("submit", $_POST)) {  
        if($secret == $_POST['secret']) {  
        print "Access granted. The password for natas7 is <censored>";  
    } else {  
        print "Wrong secret";  
    }  
    }  
?>  
  
<form method=post>  
Input secret: <input name=secret><br>  
<input type=submit name=submit>  
</form>  
  
<div id="viewsource"><a href="index-source.html">View sourcecode</a></div>  
</div>  
</body>  
</html>
```
### The PHP Code
The interesting part of this code is within the content div. There we see embedded php code (`<? ... ?>`). 
Of course understanding php helps in this scenario but even without detailed php knowledge we can get a rough understanding of what's going on. For the details it is always a good idea to check the official documentation. 
Here an annotated version of the code:
```php
include "includes/secret.inc";  #includes the content of the specified file
  
    if(array_key_exists("submit", $_POST)) {  #checks if the key "submit" is contained in the _POST array
        if($secret == $_POST['secret']) {  #cecks if the value with the key "secret" contained in the _POST array is equal to the value of a variabel named secret
        print "Access granted. The password for natas7 is <censored>";  
    } else {  
        print "Wrong secret";  
    }  
    }  
```

> **HINT** $\_POST — HTTP POST variables

So in other words the code checks if a HTTP POST request contains a submit. If yes then the value secret is checked against a variable. If those two are equal then a string containing the password is printed. 
This is already very helpful and leaves us with only a few open questions:
- which value is contained in `$_POST['secret']`?
- which value has `$secret`?
### The HTML Code
Now to answer the first of the questions lets take a look at the HTML code of the form:
```
<form method=post>  
Input secret: <input name=secret><br>  
<input type=submit name=submit>  
</form>
```

Here we can see that whatever we put in the textbox will be send to the server as POST variable 'secret'
### The secret.inc
No to answer the last question - which value is assigned to `$secret` - we need to check out the included file. Since there is no variable declaration or assignment in the php snippet directly it has to be in the included file. So lets browse to 'http://natas6.natas.labs.overthewire.org/includes/secret.inc'
![Pasted image 20231003115821.png](/Pasted%20image%2020231003115821.png)
## Putting it all together
Let's take a step back and put all the pieces together. 
We have a variable with the predefined value of 'FOEIUWGHFEEUHOFUOIU'. We have an input field and whatever is put into that field is compared to the predefined variable. We want that comparison to evaluate to true.
Easy as that:
![Pasted image 20231003120032.png](/Pasted%20image%2020231003120032.png)
And the result of that is....

![Pasted image 20231003120107.png](/Pasted%20image%2020231003120107.png)

## Further Reading
https://www.php.net/manual/en/reserved.variables.post.php
https://www.w3schools.com/php/php_superglobals.asp


---
title: "Natas8"
date: 2023-11-15T18:49:20Z
draft: false
toc: false
images:
tags: 
  - untagged
---
# Natas 8


| url | http://natas8.natas.labs.overthewire.org/ | 
|---| -- |

> **Keywords\*:** encoding
> *\*  if you are stuck with this and don't know where to start or even look, you can research the keywords instead of reading the whole write-up. This will give you the chance to solve the challenge yourself with just a small hint *

 **Questions to ask:**
> what happens with the user input on the serverside?
> how can we use the input to trigger unwanted behaviour on the server?
## Getting an overview
How do we start? You should really know that by now - by looking at the rendered webpage.
![Pasted image 20231003171036.png](/Pasted%20image%2020231003171036.png)

Similar to [Natas 6](Natas%206.md) the webpage consists of a form that we can input text in and submit and a link to view the sourcecode. So let's do that.
## Digging deeper

```html
<html>   <head>   <!-- This stuff in the header has nothing to do with the level -->   <link rel="stylesheet" type="text/css" href="http://natas.labs.overthewire.org/css/level.css">   <link rel="stylesheet" href="http://natas.labs.overthewire.org/css/jquery-ui.css" />   <link rel="stylesheet" href="http://natas.labs.overthewire.org/css/wechall.css" />   <script src="http://natas.labs.overthewire.org/js/jquery-1.9.1.js"></script>   <script src="http://natas.labs.overthewire.org/js/jquery-ui.js"></script>   <script src=http://natas.labs.overthewire.org/js/wechall-data.js></script><script src="http://natas.labs.overthewire.org/js/wechall.js"></script>   <script>var wechallinfo = { "level": "natas8", "pass": "<censored>" };</script></head>   <body>   <h1>natas8</h1>   <div id="content">      

<?      

$encodedSecret = "3d3d516343746d4d6d6c315669563362";      

function encodeSecret($secret) {       
	return bin2hex(strrev(base64_encode($secret)));   
}      

if(array_key_exists("submit", $_POST)) {       
	if(encodeSecret($_POST['secret']) == $encodedSecret) {       print "Access granted. The password for natas9 is <censored>";       
	} else {       
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
### The php code
We can see that php code is embedded into the HTML. Let's take a closer look at that part

```php
<?      

$encodedSecret = "3d3d516343746d4d6d6c315669563362"; # a variable definition. The name implies that this is our secret but encoded in some form 

function encodeSecret($secret) {       # a function that takes one parameter, performs some operations and returns it
	return bin2hex(strrev(base64_encode($secret)));   
}      

if(array_key_exists("submit", $_POST)) {       # check if a key called 'submit' is contained in the POST array
	if(encodeSecret($_POST['secret']) == $encodedSecret) { # check if the return value from encodeSecret with the value of 'secret' from the POST array is equal to the value of the encodedSecret variable
		print "Access granted. The password for natas9 is <censored>";       
	} else {       
		print "Wrong secret";       
	}   
}   
?>
```

Now what is the difference to level six where we could simply read the plaintext password from an imported file? In this case we have the correct password already in the code, but it is not plaintext. Instead it is stored encoded. But we also have the function that was most probably used to encode it:

```
function encodeSecret($secret) {       
	return bin2hex(strrev(base64_encode($secret)));   
} 
```

This function is a little less-readable due to it performing multiple actions in one line. However, reading it from the inside-out we get an understanding of the operations:
1. the given value is base64 encoded
2. the string is reversed (read the documentation if you are not sure what `strrev` does)
3. the binary data is converted to hex
4. *The result is returned*

Now what exactly does this mean? 
Viewing this function in the context that it is called (the second if-statement) we get the following process:
Whatever the user puts in the textbox is encoded following the steps mentioned above. Then the encoded userinput is compared to the encodedSecret. If those values are the same, we are given the password of Natas 9.
### Reversing the Encoding
Now trying out all potential passwords in an attempt to brute-force it could work. However since the encoding is very weak we could simply reverse it to get the 'plaintext' value of encodedSecret.

To do this we can use cyberchef.io and simply reverse the steps taken by the encodeSecret function:
1. convert hex to binary:
   ![Pasted image 20231003173102.png](/Pasted%20image%2020231003173102.png)
2. Reverse the string
   ![Pasted image 20231003173151.png](/Pasted%20image%2020231003173151.png)
3. base64 decode
   ![Pasted image 20231003173240.png](/Pasted%20image%2020231003173240.png)
The result is the value 'oubWYf2kBq'. Now if our assumptions were correct, this should be our correct password and yield us the credentials of natas9:
![natas_8_result.png](/natas_8_result.png)
## Further Reading
https://www.geeksforgeeks.org/difference-between-encryption-and-encoding/


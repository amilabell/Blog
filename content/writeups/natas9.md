---
title: "Natas 9 -> Natas 10"
date: 2023-11-15T18:49:23Z
draft: false
toc: false
images:
tags: 
  - untagged
---

| url | http://natas9.natas.labs.overthewire.org/ | 
|---| -- |

> **Keywords\*:** input sanitation, command injection
> *\*  if you are stuck with this and don't know where to start or even look, you can research the keywords instead of reading the whole write-up. This will give you the chance to solve the challenge yourself with just a small hint *

 **Questions to ask:**
> what happens with the user input on the serverside?
> how can we use the input to trigger unwanted behaviour on the server?

## Getting an overview
You know the drill. Let's take a look at what we got.
![Pasted image 20231004135452.png](/Pasted%20image%2020231004135452.png)
Similar to [Natas 8](../natas8) we get an input field and a link to the sourcecode. This time however we are not asked to provide a secret. Instead the input field seems to be some kind of search. Let's try it out:
![natas9_trying_out_search.png](/natas9_trying_out_search.png)
Interesting! Apparently this website is some kind of dictionary search. Let's check out the clientside code first, before checking out the linked serverside sourcecode:
![natas9_clientside_code.png](/natas9_clientside_code.png)
Nothing too interesting in here. But we note the weird name 'needle' assigned to the input field.
Now that we have gotten a rough overview of this webpage let's check out the sourcecode linked.
## Digging deeper
As in the previous level, the sourcecode contains embedded php code:
```html
<html>   <head>   <!-- This stuff in the header has nothing to do with the level -->   <link rel="stylesheet" type="text/css" href="http://natas.labs.overthewire.org/css/level.css">   <link rel="stylesheet" href="http://natas.labs.overthewire.org/css/jquery-ui.css" />   <link rel="stylesheet" href="http://natas.labs.overthewire.org/css/wechall.css" />   <script src="http://natas.labs.overthewire.org/js/jquery-1.9.1.js"></script>   <script src="http://natas.labs.overthewire.org/js/jquery-ui.js"></script>   <script src=http://natas.labs.overthewire.org/js/wechall-data.js></script><script src="http://natas.labs.overthewire.org/js/wechall.js"></script>   <script>var wechallinfo = { "level": "natas9", "pass": "<censored>" };</script></head>   
<body>   
<h1>natas9</h1>
<div id="content">
<form>
Find words containing: <input name=needle><input type=submit name=submit value=Search><br><br>
</form>
Output:
<pre>
<?   
$key = "";
if(array_key_exists("needle", $_REQUEST)) {
	$key = $_REQUEST["needle"];
}      
if($key != "") {
	passthru("grep -i $key dictionary.txt");
} 
?>   
</pre>
<div id="viewsource"><a href="index-source.html">View sourcecode</a></div>
</div>
</body>
</html>   
```
The code is getting a bit more complicated and it is worth analysing it in more detail.

## Analysing the PHP code
The PHP code (this time enclosed in `<pre/>` tags which we will ignore) works with one variable called key that is initialized with zero:
```php
$key = "";
```

The code consists of two if statements. 
The first one is similar to the ones we have seen in [Natas 8](../natas8).
```php
if(array_key_exists("needle", $_REQUEST)) {
	$key = $_REQUEST["needle"];
}  
```
It checks whether the key 'needle' exists in an array. This time the array is not \_POST but \_REQUEST. The details about the differences are not too important in this context. It is only necessary to understand that the \_REQUEST also contains the contents of \_POST and \_GET.
If the condition in the if statement is true ('needle' exists) then the value corresponding to the 'needle' key is assigned to the key variable.
So what is this 'needle' key-value pair?
Since the \_REQUEST gets populated by the client and we have already noticed that the input field is assigned the 'needle' key, we now know that this contains whatever the user puts in the textfield (in the example above the word 'dog').

Nearly done, only one more if statement to understand:
```php
if($key != "") {
	passthru("grep -i $key dictionary.txt");
} 
```
The code checks if key is empty. Since we already know that key is assigned whatever the user entered, we know that it is only empty if the user left it empty and pressed the 'search' button.
If the key **is not** empty a function called `passthru` is called. We can see that as input the function takes something that looks like a \*nix command.
Something like this should always spark our interest. Is this executing commands on the webserver? Let's check the documentation.

>The **passthru()** function is similar to the [exec()](https://www.php.net/manual/en/function.exec.php) function in that it executes a `command`. This function should be used in place of [exec()](https://www.php.net/manual/en/function.exec.php) or [system()](https://www.php.net/manual/en/function.system.php) when the output from the Unix command is binary data which needs to be passed directly back to the browser. (source: https://www.php.net/manual/en/function.passthru.php)

## Playing with passthru()
So now we know that part of the PHP code is executing a command and sends the output of that command back to the browser. We can also see that the command executed is 
```
grep -i $key dictionary.txt"
```
That contains the value of key which is controlled by our input!
So let's ask ourselves what could we possibly do with that?
Can we insert a string that is interpreted as another \*nix command?
Lets try using '; cat' as input into the textbox. The command executed should then be
```
grep -i ; cat dictionary.txt
```
The expected result is that all contents of 'dictionary.txt' are returned. And when we try it out, we see that this is exactly what's happening
![natas9_trying_out_ci.png](/natas9_trying_out_ci.png)

## Profit (aka exploiting passthru())
So what we have here is seemingly arbitrary command execution on the webserver itself. How can we use this to get the password of natas10? Well we know that it is stored in `/etc/natas_webpass/natas10` and we have the ability to use `cat` to output files. 
Let's combine those two by crafting a specific input for our textbox:
```
; cat /etc/natas_webpass/natas10
```
The command executed by passthru() should then be
```
grep -i ; cat /etc/natas_webpass/natas10 dictionary.txt
```
And lo and behold, it works:
![natas9_successful_cli.png](/natas9_successful_cli.png)⏎   

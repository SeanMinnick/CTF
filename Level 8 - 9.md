Level 8-9 loads into a page with a "submit secret" box and a link to view the source code. I submit the word "secret" just in case but it obviously doesn't work. When you click on view sourcecode it shows you a php function that authenticates the input.

```
`<?      
$encodedSecret = "3d3d516343746d4d6d6c315669563362";    

function encodeSecret($secret) {       return bin2hex(strrev(base64_encode($secret)));
}      
if(array_key_exists("submit", $_POST)) {       if(encodeSecret($_POST['secret']) == $encodedSecret) 
	{       print "Access granted. The password for natas9 is <censored>";       } else {       
	print "Wrong secret";       
	}   
}   

?>`
```

now all you have to do is reveres the encodeSecret function to know what you need to submit

the string is clearly in hex, so i put it in cyberchef and did the opposite of the function, using From Hex, Reverse, and From Base64 to get a small string "oubWYf2kBq" Inputting this gives u the password to Natas9
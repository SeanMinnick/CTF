This challenge has a basic submit function to search for what words contain whatever string you input. The source code is php and is shown to you in the challenge.

```
`<?   
$key = "";  

if(array_key_exists("needle", $_REQUEST)) {    
	$key = $_REQUEST["needle"];   
	}      
	
if($key != "") {
	passthru("grep -i $key dictionary.txt");   }   
?>`
```

when you submit in the html <form> that takes your input, it is assigned to "needle" 

this means that the function takes your input, runs it through request and assigns it to key. the next if statement runs if key is assigned to any input, and puts your input into a basic grep function.

admittedly focused on so many wrong things first, but the vulnerability here is that your input is placed directly into a serverside linux command. the first test I did for this was putting 'cat dictionary.txt' into the input. This didn't cat all of dictionary.txt, however it did give a large output showing that command injection here was possible

the next step was to see my own command, and placement on the server. I inputted 'a; pwd' this ends the grep function with 'a;' and moves to my own command pwd. This showed my current filepath which was '/var/www/natas/natas9'

we know from the last challenge that the passwords are stored in /etc/natas_webpass. So the next step was to input the following 

a; cd ../../../../etc/natas_webpass; cat natas10

this prints natas10 in natas_webpass and gives you the password for the next level



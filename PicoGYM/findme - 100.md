#Description 
	Help us test the form by submitting the username as `test` and password as `test!`
	Additional details will be available after launching your challenge instance.


This challenge had a lot of false leads to make you miss how easy it really is. There is a login page where you are instructed to input test and test! to make it to another page where you can "Search for the flag" with a basic search functionality.

My first instinct was to try SQL injection on the flag search page, as it's a common challenge when you just have a plain search functionality, but I didn't get anything, and the source code showed a super basic search function that didn't seem to call to anything. Similarly using inspect it didn't look like the site was calling out to anything externally, and it didn't show it having any source code other then base page.

After that I defaulted to looking at the HTTP requests with burpsuite to see if anything popped up that was out of the ordinary when logging in with test test!
There was multiple steps when logging in, and some GET requests that contained what looked like base64. When you intercept with burp you can forward each request one at a time, which showed the browser going to two different locations titled "flag", that contained the random base64. 

Concatenating the two base64 strings and decoding in CyberChef gave me the flag. 

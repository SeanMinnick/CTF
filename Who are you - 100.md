
#Description 
	Let me in. Let me iiiiiiinnnnnnnnnnnnnnnnnnnn


This challenge leads to a website with a gif, and a title that says only people using the official PicoBrowser can access this site. I was pretty locked up at this point looking at the basic GET request in BurpSuite to see what I could do. When I modify the User-Agent header in burp and forward the request you end up getting a new screen that says "I don't trust users from another site"

This implies that it should be referred by itself. I added a Referer header with it's own link as the data, and that was what it was looking for. The next level said "This site only worked in 2018"

This next stage is referencing a Date header which is absent in the initial GET request. The format of a Date header is odd, so I found an example online, and swapped the year for 2018. Adding the date header, and referer header allows you to get to the next stage.

The next stage was a lot more interesting, all it said was "I don't trust users who can be tracked." I expected this to be a proxy challenge, adding multiple IP's into a header. It was much easier then that, there's apparently a header called DNT for do not track. Setting it to 1 and adding it alongside the Date and Referer header allowed me to move on to the next stage. 

The next page said "This page is only for people from Sweden." This was definitely the hardest stage because the header I needed to add wasn't as easy figure out. The X-Forwarded-For header is what I needed to specify a client IP. Setting this IP to an address from Sweden is all I needed to fake that I was in Sweden. I went online to find a random Swedish IP, and put it alongside all of the other headers to move on to the final stage. 

The last stage just said "You're from Sweden, but don't speak Swedish?" I just had to look up the two character code for Swedish and add it to the accept-language header to get the final flag.



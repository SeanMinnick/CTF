#Description 
	The web project was rushed and no security assessment was done. Can you read the /etc/passwd file?
	Additional details will be available after launching your challenge instance.

This a beautiful challenge. You launch onto a really basic website, with almost no room for interaction. The only interactable feature are small buttons that give added details of each mentioned item. These don't redirect you, or move you to any other page. The only two real hints about how you get the passwd file from this are the title of the challenge and one of the tags. SOAP refers to Simple Object Access Protocol. But more importantly, one of the tags on the challenge is XXE, which is External Entity Injection, an XML attack.

The only interactable item was the details buttons, so I intercepted the POST request with burp and was able to see the XML that grabs the details. This is where I hit a small roadblock. initially I was under the impression that I would have to use an Xinclude attack, since I didn't believe I could include a DOCTYPE call in the xml I had available. This was simply because I didn't have xml tags around the XML block in the POST request. I went through the portswigger page on XXE and modified their Xinclude payload for the site. 

```
<foo xmlns:xi="http://saturn.picoctf.net:64275/XInclude">
<xi:include parse="text" href="file:///etc/passwd"/></foo>
```

I tried using this payload idea inside the id tags in the POST request, to make it parse this payload as the id of the details. This didn't work, and in hindsight it makes sense why this wouldn't be the solution. I didn't even try the more basic type of XXE using DOCTYPE because of an assumption I had on XML, something I've barely touched in the past. Still wasted time trying to use xinclude though, and I learned a lot in this small tangent. 

After this I did some more research on XXE and came to my senses. 

```
<!DOCTYPE foo [
   <!ENTITY xxe SYSTEM "file:///etc/passwd" >]>
   <id>&xxe;</id>
```

This is what I used to get the flag. I modified a few different payloads to get this which fit the id tags of this specific challenge. I added the xxe placeholder and called it with the DOCTYPE flag. Initially I was trying to put all of this inside the data tags which contain the id tags. Which yielded no results, so finally I placed the DOCTYPE and ENTITY lines outside the data tags, and replaced the id number with the xxe placeholder. This read the entire passwd file and gave me the flag. 

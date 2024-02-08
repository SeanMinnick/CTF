
#Description:
	I sent out 2 invitations to all of my friends for my birthday! I'll know if they get stolen because the two invites look similar, and they even have the same md5 hash, they tey are slightly different! You wouldn't believe how long it took me to find a collision. Anyway, see if you're invited by submitting 2 PDFs to my website. 

This challenge leads to a website with two file inputs, where it requires you to upload two pdfs.

From the hints in the description it's clear that they're looking for two files that have the same hash, to emulate this situation. In the title the word Birthday is capitalized, which usually means its a hint, so I googled "MD5 collision "birthday"" and got an article talking about the birthday paradox and how it relates to hashing algorithms 

https://auth0.com/blog/birthday-attacks-collisions-and-password-strength/

This confirmed my thoughts on wanting to submit a collision of two PDFs. I started doing some research on how you can create a collision leading me to a GitHub page talking about collisions in different hashing algorithms.
https://github.com/corkami/collisions

there is so much elaborate information on this README, it's insanely in depth, but for this challenge all I need is two pdfs that are different, but collide, and in this repo he provides examples. 
I downloaded the two different pdf examples, and uploaded them both to get the flag. 
https://github.com/corkami/collisions/tree/master/examples/free

When your submitted pdfs collide, it shows the index.php file that also has the PHP on how it checks the hash. 


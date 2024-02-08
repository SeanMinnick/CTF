#Description
	This website can be rendered only by **picobrowser**, go and catch the flag! `https://jupiter.challenges.picoctf.org/problem/28921/` ([link](https://jupiter.challenges.picoctf.org/problem/28921/)) or http://jupiter.challenges.picoctf.org:28921

I had an unfair advantage in this challenge because I'm doing all of these in PicoGYM and not the years they were in competition. I recently did the "Why are you" challenge, also documented here, which used a similar method in a long chain of attacks to get the flag. The second I saw that you needed to use picobrowser I could ignore all of the decoys in this challenge.

When you load in theres a huge green button that says flag, and it's really that simple. You intercept the http request when you hit the button and change the browser to "picobrowser" and you get the flag. 
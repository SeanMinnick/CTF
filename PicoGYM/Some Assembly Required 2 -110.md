
There was no description for this challenge, it takes you to a webpage that has an input flag field and that's it. From the title I assumed this would be a challenge having to do with assembly which was very intimidating. Reloading the page in inspector I see it calling to an assembly file which I downloaded and ran the file command and the strings command on. This let me know that this was a WebAssembly file, with a 'checkflag' function.

I started looking up how to reverse engineer wasm and realized I was pretty screwed. I started trying to go through the assembly and figure out how this function worked in the decompiled version in the firefox debugger. I was lost. However, there was a large string in the bottom of the file, which I assumed meant something, but I had no clue what it was encoded with. 

I threw it into CyberChef and ran magic on it to try to brute force whatever it was encrypted with, figuring out it was XOR with a key of 8 and got the flag. I don't think this is how the challenge is supposed to be done if I'm being honest. This didn't take as long as it should have for only having 5,000 solves on picoGYM. I looked up a writeup on it to see if other people took this basic approach or if I completely evaded the point of the challenge.

One person in the write up decompiled it multiple times with different tools, to read the function, and was able to know it was XOR with a key of 8, so I feel kind of cheap for how I got this one, but we'll take it. CyberChef is just too good. 

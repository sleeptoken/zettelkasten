
2025-05-01 20:52

Source: #reversing 

Overall structure is similar to [[XOR (known key) values stored at a address]] 

The structure of this code looks very similar to the last challenge, with the two calls to `GetStdHandle()`
 - a `WriteFile()` call to display the prompt
 - a `ReadFile()` call to collect the input to a buffer (at 0x402159 in this case). 
 Where the two programs differ is that this challenge performs a function call (at address 0x40105F) to determine its success or failure condition whereas the last challenge performed a loop directly in this function.


### References
Flare-on 2 2015

[Flare-On-Challenges/Write-ups/2015/2015solution2.pdf at master · fareedfauzi/Flare-On-Challenges · GitHub](https://github.com/fareedfauzi/Flare-On-Challenges/blob/master/Write-ups/2015/2015solution2.pdf)
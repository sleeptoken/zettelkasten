
2025-05-01 20:52

Source: #reversing 

Overall structure is similar to [[XOR (known key) values stored at a address]] 

The structure of this code looks very similar to the last challenge, with the two calls to `GetStdHandle()`
 - a `WriteFile()` call to display the prompt
 - a `ReadFile()` call to collect the input to a buffer (at 0x402159 in this case). 
 Where the two programs differ is that this challenge performs a function call (at address 0x40105F) to determine its success or failure condition whereas the last challenge performed a loop directly in this function.

```Dissassembly of the call to check function
text:0040104E      lea    eax. [ebp-4] 
text:00401051      push   eax             :1pNumberOfBytesWritten
text:00401052      push   11h             :nNumberOfBytesToWrite 
text:00401054      push   dword ptr [ebp-4] 
text:00401057      push   offset unk_402159 
text:0040105C      push   dword ptr [ebp-18h] 
text:0040105F      call   sub_401084
``` 

 In this block of code we see the call to the function whose return value (zero or non-zero) will determine if the program displays the success or failure message. This function takes 3 arguments: 
 - The pointer value (0x4010E4) from the first call instruction (stored at cbp-10h),
 - A pointer to buffer of input (0x402149)
 - The number of bytes entered as input (stored in ebp-4).
 
  Let's take a look at that function! Since we have already determined what the function parameters are I took the liberty of renaming them in the check function to "key", "input", and "dwinputLength". The first sequence of instructions of note in this function begin at 0x40108E:

```
mov ecx. 25h cmp [ebp+dwInputLength], ecx loc_401096: short loc_401007
```
### References
Flare-on 2 2015

[Flare-On-Challenges/Write-ups/2015/2015solution2.pdf at master · fareedfauzi/Flare-On-Challenges · GitHub](https://github.com/fareedfauzi/Flare-On-Challenges/blob/master/Write-ups/2015/2015solution2.pdf)

2025-04-16 09:01

Source: #reversing 
### TL;DR

exe file asks us to enter password; user entered password is XOR'd and compared with values stored in a buffer 
if pass is correct we get success message. 
### REAL DEAL 

Opening the program in IDA we see we only have one Function 
The First 2 API calls it makes are to a function called `GetStdHandle()`, which will return a handle to one of the 3 standard IO streams for the process (Standard, input, output and error)

After each call, the `eax` register is moved to a stack variable. Since `eax` represents the return value from a function after a call, this register contains the handle values for the standard handles after each call, and they are being saved on the stack. 

The next 2 API functions called in the program are `WriteFile()` and `ReadFile()` 

1. The call to `WriteFile()` takes 5 arguments and these correspond to the 5 push instructions preceding the call. This call is simply writing the prompt message to the standard output handle, which causes it to be displayed to the user in the terminal. 

2. The next call to `ReadFile()` is more interesting. It also takes 5 arguments, starting with the file handle to read from, the buffer to store the data, and the maximum amount to read. The program is reading at most 50 bytes into a buffer at memory address `byte_402158`

Further there are 2 blocks telling us weather our input was correct or not. The selection of which block to execute occurs by the `jl` instruction (jump if less than) at memory address `0x401061` or potentially by the `jnz` instruction (jump if not zero or not equal) at memory address `0x40105B`

Not shown in this fragment is the instruction "`xor   ecx, ecx`" which simply sets the `ecx` register to zero prior to the beginning of the loop. We can tell that this is a loop because there is a thick, backward pointing arrow in our disassembly graph. The `JL` instruction at the end jumps back to the beginning of the loop if ecx is less than `18h` (`24` in decimal), otherwise the code will execute the success block. 

The `ecx` register is incremented before this comparison, and it started out the loop as the value 0, so it is likely the loop counter. Therefore this program will only print the success message if this loop iterates a full 24 times. 

The loop can short circuit, however. In each iteration of the loop, a byte is loaded from the input buffer in the `al` register, then XOR'd with the value `0x7D`, then compared against a byte in another array at memory address `0x402140`. If the bytes match then the zero flag will be set, causing the `jnz` instruction to not branch. If the bytes did not match (i.e. you entered a wrong digit in the password) then the zero flag will not be set, causing the branch to the failure bock to occur. 

Since the program is performing a single byte XOR loop. If you then XOR the bytes found at memory address 0x402140 with the value 0x7D, the correct answer will be revealed. 

```python
#!/usr/bin/env python
s = [0x1F,0x8,0x13,0x13,0x4,0x22,0x0E,0x11,0x4D,0x0D,0x18,0x3D,0x1B,0x11,0x1C,0x0F,0x18,0x50,0x12,0x13,0x53,0x1E,0x12,0x10]
print(''.join([chr(i ^ 0x7d) for i in s]))
```

### References
Flare-on 1 2015

https://adamhlt.com/flare-on-2015-0x1-first-challenge-writeup/#password-recovery
https://github.com/fareedfauzi/Flare-On-Challenges?tab=readme-ov-file
https://github.com/fareedfauzi/Flare-On-Challenges/blob/master/Write-ups/2015/2015solution1.pdf
https://www.aldeid.com/wiki/The-FLARE-On-Challenge-2015/Challenge-1
https://secwriteups.blogspot.com/2016/08/flare-2015-challenge-1.html


https://www.reddit.com/r/Assembly_language/comments/1855gmj/what_does_edxecxebx_or_eax_mean_in_assembly/
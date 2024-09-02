
2024-08-24 16:30

Source: #htb #CTF 

Tags: [[Binary Exploit]]
### Challenge

The program expects a single argument with a password

Looking at the `main` function, we can see some Unix signals trickery at the beginning, and the endless loop with the `invalidInstructionException();` in the end.

While selecting the `invalidInstructionException();` instruction in the decomplication, Ghidra highlights the following assembly instruction:
```nasm
001012e6	0f 0b	UD2
```

Referring to [Intel® 64 and IA-32 Architectures Software Developer Manual, Vol. 2B](https://www.intel.com/content/www/us/en/developer/articles/technical/intel-sdm.html), `UD2` “generates an invalid opcode exception. This instruction is provided for software testing to explicitly generate an invalid opcode exception. The opcodes for this instruction are reserved for this purpose.”  
[[Ghidra]] stops disassembling the file after the first occurrence of `UD2`. It makes total sense, by default the whole program will be terminated due to receiving a `SIGILL` Unix signal. 

 - let’s select the rest of the code that hasn’t been disassembled and disassemble it by pressing “D”. 
 - Next, let’s replace all occurrences of the `UD2` instruction with a `NOP`
 - After doing so, we can immediately see that there are 4 `strcmp`'s comparing the user input with string constants. And the flag seems to be a concatenation of these constants
### Alternate

open the file using `xxd` 
After that it says `Itz._0n.Ly_.UD2`. I am not sure why those dots are there but if you remove them and try to use the cleaned up string `Itz_0nLy_UD2` as the password you will get the flag which is basically the same string.
## But how does it work?

### References
https://app.hackthebox.com/challenges/Behind%20the%20Scenes
writeups- 
https://agarmash.com/posts/htb-behindthescenes-writeup/
https://publish.obsidian.md/dodofarm/Cybersecurity/Hack+The+Box/Challenges/Reversing/Behind+The+Scenes/Behind+The+Scenes+-+Writeup
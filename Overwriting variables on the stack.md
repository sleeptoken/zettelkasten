
2024-06-26 19:15

Source: #youtube 

Tags: [[Binary exploit toolkit]] [[Binary Exploit]]

![[buffer-overflow-diagram.png]]
if a buffer is overflowed then it tried to go up and overwrites everything that comes in its way
### Dynamically linked 

we have these functions here like `printf` and `gets` which are in the `lib c` library so rather than including all of the code for these functions in this file it's simply going to look out to the `lib c` library that's on the computer system that's been run on to see what is the location of this function and it can call it from there
### [[Compiling binary]] w and w/o protections

with protection 
```
gcc vuln.c -o vuln -fstack-protector-all
```
above code will output a 64 bit file 
vuln.c is the input file that needs to be compiled 
vuln is the output file 

without protection
```
gcc vuln.c -o vuln -fno-stack-protector -z execstack -no-pie -m32
```
`-m32` --> above code will output a 32 bit file  
`-fno-stack-protector` --> no protections enabled, prevent it from adding canaries to the stack
`-z execstac` --> makes sure the program is marked as executable - NX Disabled 
`-no-pie` --> means that the program will load at the same memory address each time



### References
https://www.youtube.com/playlist?list=PLHUKi1UlEgOIc07Rfk2Jgb5fZbxDPec94
https://github.com/Crypto-Cat/CTF/tree/main/pwn/binary_exploitation_101

buffer overflow - https://avinetworks.com/glossary/buffer-overflow/
Disassembler - [[Ghidra]]/IDA/Radare/BinaryNinja/Hopper : https://gist.github.com/liba2k/d522b4f20632c4581af728b286028f8f
Debugger - [[GDB]]-PwnDBG/GEF/PEDA: https://infosecwriteups.com/pwndbg-gef-peda-one-for-all-and-all-for-one-714d71bf36b8

cheatsheet - 
ghidra - https://ghidra-sre.org/CheatSheet.html

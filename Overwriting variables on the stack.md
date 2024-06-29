
2024-06-26 19:15

Source: #youtube 

Tags: [[Binary exploit toolkit]] [[binary exploit]]

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

tutorials - 
Pwn.College: [https://pwn.college](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqbV9raTJiX0dEOXRFMUtSTzFJZFVIRmZsOVgwUXxBQ3Jtc0trdUlvWkpxaEtNbVhaazN1OWMxbWdZYjBLdEVOR3hib0hCT0ZVNDIzWnFUdlVKcERSYnFaX3ZPRU45ZW1yMTkwaVp2SEVmWkFWbFdZeDJ3a2F4cklxR1RaN0NEMHRCbjBtSFpjSjBTMVdxanFTYW9YTQ&q=https%3A%2F%2Fpwn.college%2F&v=wa3sMSdLyHw)
How2Heap: [https://github.com/shellphish/how2heap](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqazdZYTRFUzR3clZRUFh6NVlBQmx1UXE0TFJyQXxBQ3Jtc0tsZEpybkMwWmRXRkE1UmJQaGtXX1g3enh6bGdDWmFSSWl4QmtIY29sV045VGpJbk1tcmtPUmhTeTJPYlZ5M0hOVGhqRTJFX3ZBRWNxeEtpOTdwZjNCVEFwS2ZLUVI1aTFRY2NmNEdCNXpnX2FfaDc3SQ&q=https%3A%2F%2Fgithub.com%2Fshellphish%2Fhow2heap&v=wa3sMSdLyHw) 
NightMare: [https://guyinatuxedo.github.io](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqa0U4TEF6T0hnSDVkUHNhR0wxWklwMjRrQlpOUXxBQ3Jtc0tuZ2V1MmNIMVVhbGNMMTFMYXMtUF8xOWxUNVFpLVpteVZlY1R3cU1INmN0NDdnbmlKTDZvNW03Y0JqQ2FCN0wxZnlvcXlnZDNPWTVEaUw2dFNDMXEtNkVlbGsyellpVTFSSmU3S1ptTFhOaERJTW5WUQ&q=https%3A%2F%2Fguyinatuxedo.github.io%2F&v=wa3sMSdLyHw) 
Ir0nstone: [https://ir0nstone.gitbook.io/notes/ty...](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqbURYLXJ2Q08zQVBtaHZPSlRub2dGbkxBNkttUXxBQ3Jtc0ttRXFMLWlDamp5RUljUXBfYnJPRjIyQlJDX3NFVVZXVEhCcW9Rb29PM2R6dWVLdGFST1l3cVNDQzhqdFNqTll5TDhXNHlxWE13M1h2VmJMd2I4S010NzdOcG1BcVh1UDZjOU5HdzY1YVpkQ3VVMm9IOA&q=https%3A%2F%2Fir0nstone.gitbook.io%2Fnotes%2Ftypes%2Fstack&v=wa3sMSdLyHw) 
PinkDraconian:    [![](https://www.gstatic.com/youtube/img/watch/yt_favicon.png) • Pwn Zero To Hero](https://www.youtube.com/playlist?list=PLeSXUd883dhjmKkVXSRgI1nJEZUDzgLf_)   
More: [https://github.com/Crypto-Cat/CTF#readme](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqbnhoMTY0a08zTjFTTXduZlFrakxJQkFDbXR2UXxBQ3Jtc0trR3BZemxLVE1DNFlMQzQ3bDgta2U3U29Db2NFU2xEcVlQdEl5RWZ3V3MzM0RWTjJyNDdqcU1kV3c5VmY4QngxSHhYVXhPVjJ6anVZSTRhMGlBLWlZWkQxMWJHN3FRbWwyRElLb1dzaEFWUmJvYVdsRQ&q=https%3A%2F%2Fgithub.com%2FCrypto-Cat%2FCTF%23readme&v=wa3sMSdLyHw)
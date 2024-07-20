
2024-07-06 13:03

Tags: [[Binary Exploit]]

**PARTIAL RELRO**  -->  if this was set to full RELRO we wouldn't be able to overwrite the global offset table address this basically defines the readability and writability of the global offset table

**STACK CANARY FOUND** 
**NX ENABLED**
**NO PIE**

 we're going to use a format string vulnerability but there's going to be no buffer overflow like list time [[Leak PIE & Ret2Lib-C]] . we're going to use the format string write operation and we're going to use that to overwrite an element of the global offset table

we want to overwrite an address of one of these functions (eg. `printf`) in the global offset table with a function that we want to call so for example the system function in `libc` so for example we've got this `printf`, if we were to enter in a value in the `printf(buffer)` to overwrite an element of the global offset table we could  find the address of [[printf]] in the global offset table and overwrite it with the address of system, and because this is a continuous loop (problem specific) what's going to happen is it's going to loop around again, it's going to take an input from us and then instead of calling `printf` with the buffer it's going to call system with the buffer so if we just enter `/bin/sh` this is going to call `/bin/sh`

further to print the correct offset to write do the following - 
what you're looking to find out is the characters that you enter, what place do they get put to on the stack because that's the address that can then be used in the remainder of the right operation
##### Finding manual exploit - 
https://github.com/Crypto-Cat/CTF/blob/main/pwn/binary_exploitation_101/09-overwriting_got/manual_exploit_notes.txt

in this case because it's going to be an interactive shell we need to run it a little bit differently
```
(cat manual_payload ; cat) |  ./got_overwrite
```
### References
https://www.youtube.com/watch?v=KgDeMJNK5BU&list=PLHUKi1UlEgOIc07Rfk2Jgb5fZbxDPec94&index=10

GOT & PLT
1. https://systemoverlord.com/2017/03/19/got-and-plt-for-pwning.html
2. https://ir0nstone.gitbook.io/notes/types/stack/aslr/plt_and_got

format string vulns - 
1. https://vickieli.dev/binary%20exploitation/format-string-vulnerabilities/
2. https://codearcana.com/posts/2013/05/02/introduction-to-format-string-exploits.html
3. https://axcheron.github.io/exploit-101-format-strings/  **

Pwntools doc- https://docs.pwntools.com/en/stable/fmtstr.html
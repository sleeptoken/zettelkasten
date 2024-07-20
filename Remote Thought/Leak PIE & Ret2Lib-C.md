
2024-07-05 14:15

Tags: [[Binary Exploit]]

**PIE ENABLED**
**NX ENABLED**
64 bit
### Refer the video in references for better understanding 

if we were running this against a remote server we're not going to be able to run local commands on their system until we get a shell so we need to find a way to leak a `libc` function so we can then find our way back to the base and then find our way to the functions that we're interested in bearing in mind that every system will have a different version of `libc` and they'll have different offsets

- [[ASLR]] is OS-level and applies to stack addresses generally. 
- PIE is the same concept but applied specifically to the binary 

[[printf]] is used and 2 function are defined one that uses [[gets]] and one that uses [[fgets]] 
##### For a clear understanding - https://youtu.be/NAUA1EB-TZg?t=407&si=xwCPCxDVzdWPq8QC 
watch this section of the video - Outline Attack (PIE + Lib-C Leak)

you can find the global offset table on the top right of [[ghidra]] as `.got` 

we'll see that we don't have these full addresses this time we've just got these offsets so this base address is going to be different each time but the offset (eg. 1185) are going to be the same so if we can leak out, For example all we have to do is subtract this offset (eg. 11d6) of function `A` from the value that we leak (eg. 0x00000011d6) and we're now back at the base of the binary and then we can say if we want to find the function `B` we can just add 1185 (1185 is the offset of func `B`)to that address and we're at Function B
`gdb` has a function called as `piebase` to do what is mentioned above 

we're going to overwrite the return address with first of all, a ROP chain which is going to call `puts` to leak out the global offset table puts address and then go back to the start of the vulnerable (has `gets`) function and then we're going to do another ROP chain to call system `/bin/sh`

### References
https://www.youtube.com/watch?v=NAUA1EB-TZg&list=PLHUKi1UlEgOIc07Rfk2Jgb5fZbxDPec94&index=9

Lib-C Library - [libc-database](https://libc.rip/)

exploit (pwntools) with increasing level of automation -
1. https://github.com/Crypto-Cat/CTF/blob/main/pwn/binary_exploitation_101/08-leak_pie_ret2libc/exploit.py
2. https://github.com/Crypto-Cat/CTF/blob/main/pwn/binary_exploitation_101/08-leak_pie_ret2libc/autopwn.py
3. https://github.com/Crypto-Cat/CTF/blob/main/pwn/binary_exploitation_101/08-leak_pie_ret2libc/ropstar.py
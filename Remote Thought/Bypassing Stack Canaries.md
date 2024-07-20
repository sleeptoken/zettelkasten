
2024-07-07 19:57

Tags: [[Binary Exploit]]
### 32 Bit 

**PARTIAL RELRO**
**CANARY FOUND**
**NX ENABLED** 
**NO PIE**

refer source - https://github.com/Crypto-Cat/CTF/blob/main/pwn/binary_exploitation_101/10-bypassing_canaries/canary.c  
The goal is we use the first [[printf]] to leak the canary and then we use the second [[gets]] call to overflow the buffer making sure that we overwrite the canary with the correct value and the goal of this buffer overflow is just to reach this hacked function so it's a [[Return to Win]] challenge

on checking this out in [[ghidra]] we see stack canaries are being checked in every function, if we wanted to start overwriting other variables on the stack or the return address we're going to overwrite that canary with the correct value for our buffer overflow to be successful. 

fuzz script to loop through the elements on the stack so we're using that `printf` [[Format String Vulnerabilities (printf)]] to print all the values of the stack .
in terms of the `canary`, after fuzzing we can see that it'll typically look quite random and end in `00` unlike the `libc` and stack addresses that start with `f7` and `ff` 

1. disassemble the function that main is calling  - `disassemble func_address`
2. setup a breakpoint at the address of `printf` - `break *0x0804921f`
3. run, give no input then type the`canary` command, it will show address of the found canaries
4. so print 100 elements from the stack pointer - `x/100x %esp` 
5. identify the offset of the canary by using the above output and comparing with address of found canaries

### References
https://www.youtube.com/watch?v=TOImpHQvmpo&list=PLHUKi1UlEgOIc07Rfk2Jgb5fZbxDPec94&index=11

fuzz script - https://github.com/Crypto-Cat/CTF/blob/main/pwn/binary_exploitation_101/10-bypassing_canaries/fuzz.py
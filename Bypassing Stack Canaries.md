
2024-07-07 19:57

Source: #youtube 

Tags: [[Binary Exploit]]

**32 Bit** 
**PARTIAL RELRO**
**CANARY FOUND**
**NX ENABLED** 
**NO PIE**

refer source - https://github.com/Crypto-Cat/CTF/blob/main/pwn/binary_exploitation_101/10-bypassing_canaries/canary.c  
The goal is we use the first [[printf]] to leak the canary and then we use the second [[gets]] call to overflow the buffer making sure that we overwrite the canary with the correct value and the goal of this buffer overflow is just to reach this hacked function so it's a [[Return to Win]] challenge

on checking this out in [[ghidra]] we see stack canaries are being checked in every function, if we wanted to start overwriting other variables on the stack or the return address we're going to overwrite that canary with the correct value for our buffer overflow to be successful. 

fuzz script to loop through the elements on the stack so we're using that `printf` [[Format String Vulnerabilities (printf)]] to print all the values of the stack .

`x/100x %esp` 


### References
https://www.youtube.com/watch?v=TOImpHQvmpo&list=PLHUKi1UlEgOIc07Rfk2Jgb5fZbxDPec94&index=11

fuzz script - https://github.com/Crypto-Cat/CTF/blob/main/pwn/binary_exploitation_101/10-bypassing_canaries/fuzz.py
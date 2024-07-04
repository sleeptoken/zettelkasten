
2024-07-04 09:46

Source: #youtube 

Tags: [[Binary Exploit]]

if we were doing [[Return to Lib-C]] against a real target like a remote server we wouldn't actually know what address `libc` was at or what the offsets of the different functions would be so we would need to leak those somehow that can be done with buffer overflows but we can also do that with another type of vulnerability called a format string vulnerability

in the source code `fgets` is used i.e. so the flag size is 64 bytes and `fgets` is saying only read 64 bytes from the file into that flag variable. In `fgets` we have to specify the size .

on seeing the source code it doesn't look vulnerable because `fgets` is used and we aren't reading too many bytes into these variables, the problem is the `printf` -
anytime you take input from a user and then `printf` with that input you need to specify what type of data you're expecting to be so if they're supposed to enter a string make sure that you're using the format string of the specifier format specifier to represent a string



### References
Source code  - https://github.com/Crypto-Cat/CTF/blob/main/pwn/binary_exploitation_101/07-format_string_vulns/format_vuln.c

https://www.youtube.com/watch?v=iwNYoDw1hW4&list=PLHUKi1UlEgOIc07Rfk2Jgb5fZbxDPec94&index=8
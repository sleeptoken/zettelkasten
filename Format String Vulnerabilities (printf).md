
2024-07-04 09:46

Source: #youtube 

Tags: [[Binary Exploit]] 

if we were doing [[Return to Lib-C]] against a real target like a remote server we wouldn't actually know what address `libc` was at or what the offsets of the different functions would be so we would need to leak those somehow that can be done with buffer overflows but we can also do that with another type of vulnerability called a format string vulnerability

in the source code [[fgets]] is used i.e. so the flag size is 64 bytes and `fgets` is saying only read 64 bytes from the file into that flag variable. In `fgets` we have to specify the size .

on seeing the source code it doesn't look vulnerable because `fgets` is used and we aren't reading too many bytes into these variables, the problem is the `printf` -
anytime you take input from a user and then `printf` with that input you need to specify what type of data you're expecting to be so if they're supposed to enter a string make sure that you're using the format specifier to represent a string

in this problem if we fuzz with input with normal alphabets, the input just overflows over to the next input so it doesn't cause any segmentation faults it's not overflowing the buffer

but if we use format specifiers (eg. %x, %c, %p), essentially what we're doing is we're leaking addresses off the stack

in **64-bit** we would actually want to print these as pointers to make sure we get the full 64-bit addresses, for example output of multiple `%p` might be a `libc` address then what we could do is use [[gdb]] to go and see what is this pointing to. If this is pointing to a `libc` function we can just subtract the offset to get back to the base and then we can do our Return to Libc attack

exploit code listed in the references works for 32bit as well as 64bit because we have that `context.binary` set it doesn't matter.

### References
Source code  - https://github.com/Crypto-Cat/CTF/blob/main/pwn/binary_exploitation_101/07-format_string_vulns/format_vuln.c

https://www.youtube.com/watch?v=iwNYoDw1hW4&list=PLHUKi1UlEgOIc07Rfk2Jgb5fZbxDPec94&index=8

Format String Vulns- 
1. https://vickieli.dev/binary%20exploitation/format-string-vulnerabilities/ **
2. https://codearcana.com/posts/2013/05/02/introduction-to-format-string-exploits.html
3. https://axcheron.github.io/exploit-101-format-strings/
4. https://docs.pwntools.com/en/stable/fmtstr.html

exploit(pwntools) - https://github.com/Crypto-Cat/CTF/blob/main/pwn/binary_exploitation_101/07-format_string_vulns/fuzz.py
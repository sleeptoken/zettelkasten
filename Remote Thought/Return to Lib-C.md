
2024-07-03 18:42

Source: #youtube 

Tags: [[Binary Exploit]]

# 32-Bit

**This time we have NX enabled** (we will not be able to inject shell code)
**File should be dynamically linked** 

dynamically linked means that the functions that are called from `libc` for example `gets` and `puts` rather than including all of the code in the binary that code is stored in your `libc` library on your system and whenever the program wants to access one of those functions that's going to have a look at the global offset table
here basically what's going to happen is whenever it calls ,for example `gets`, it's going to go to this global offset table section and it's going to see do we know what address the `gets` function is in the `libc` library on this computer because every version of `libc` is going to have different functions with different amounts of code so the offsets will always be different and sometimes you'll have protections like [[aslr]] on there which will actually randomize the address so it'll go and find out what the address is and it'll return and it'll populate the global offset table with that address so that the next time it's called it won't need to find out what that address is, it'll just use what it's populated in the global offset table
### Disable ASLR

on using `ldd` on the file we see that the address of `libc` keeps on changing every time we run the `ldd file_name` command this means that `ASLR` (address space layout randomization where basically binaries will have a random address to prevent buffer overflow attacks) is enabled

use `readelf` to find `system` funtion from `libc`
```
readelf -s /lib/i386-linux-gnu/libc.so.6 | grep system
```
below code gives the offset of the string - 
```
strings -a -t x /lib/i386-linux-gnu/libc.so.6 | grep "/bin/sh"
```
we gotta add the `libc` base address that we got from the `ldd` command to the address of the system function before we append the whole thing to our payload
# 64-Bit

```
ropper --file file_name --search "pop rdi"
```
above code to find address of `pop rdi`

we're going to need to pop the `/bin/sh` string into the `rdi` register so that when the system function is called it looks in the `rdi` for that string 
1. `32-bit` we will look on the stack 
2. `64-bit` we will look in these registers

further its very similar to 32-bit see the `pwntools` script mentioned in references 
### References

https://www.youtube.com/watch?v=0CFWHjc4B-I&list=PLHUKi1UlEgOIc07Rfk2Jgb5fZbxDPec94&index=7

32bit - https://github.com/Crypto-Cat/CTF/blob/main/pwn/binary_exploitation_101/06-return_to_libc/32-bit/exploit.py

64bit - https://github.com/Crypto-Cat/CTF/blob/main/pwn/binary_exploitation_101/06-return_to_libc/64-bit/exploit.py
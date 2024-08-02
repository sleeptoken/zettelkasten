
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

disassemble main function - 
Based on the decompiled code, we need to set a breakpoint after the second input on `0x00000000004015da <+469>: mov rax,QWORD PTR [rip+0x2a2f] # 0x404010 <stdout@GLIBC_2.2.5>`.

To set the breakpoint on `b *main+469`, we can execute the following commands:


### References

https://github.com/evyatar9/Writeups/tree/master/CTFs/2023-Cyber_Apocalypse_HTB/Pwn-Labyrinth
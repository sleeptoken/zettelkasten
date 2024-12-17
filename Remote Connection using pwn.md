
2024-12-17 12:51

Source: #htb 

Tags: [[pwntools]]

```
from pwn import * 
import re
context.log_level = 'ERROR' 
io = remote("94.237.61.84", 51388) # Change IP and PORT 
for i in range(110):
	io.sendlineafter(b": ", str(i).encode())
	res=io.recvline().decode() 
	match = re.findall(r'Character at Index [0-9]{1,3}: (.*)',res)
	print("".join(match),end='') 
print()
```

`context.log_level = 'ERROR'`
- This line **suppresses unnecessary logs** from the `pwntools` library by setting the logging level to `'ERROR'`.

**`sendlineafter(b": ",  str(i).encode())`**:
- Waits for the server to send data containing `": "` (a colon followed by a space).
- Once received, it sends the index `i` as input to the server, **encoded to bytes** using `.encode()`.



### References
https://infosecwriteups.com/htb-cyber-apocalypse-ctf-2024-misc-9d3d512900b4
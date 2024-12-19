
2024-12-17 12:51

Source: #htb 

Tags: [[pwntools]]

```python
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

`res = io.recvline().decode()`
- **`recvline()`**: Receives one line of data from the server.
- **`.decode()`**: Converts the data from bytes to a string for easier parsing.

`match = re.findall(r'Character at Index [0-9]{1,3}: (.*)', res)`
- **`re.findall()`**: Finds all occurrences of a pattern in the string `res`.
- **`r'Character at Index [0-9]{1,3}: (.*)'`**:
    - Matches lines that start with `Character at Index` .
    - `[0-9]{1,3}`: Matches 1 to 3 digits (e.g., 0, 12, or 109).
    - `(.*)`: Captures everything after the colon and space, i.e., the character we are interested in.
- **`match`**: A list containing the captured character(s) (only one character in this case).

### Stop Drop Roll

check out https://ctftime.org/writeup/38985

Gist of the solution is listed below 
```python
while True:
    try:
        got = r.recvline().decode()
        payload = got.replace(", ", "-").replace("GORGE", "STOP").replace("PHREAK", "DROP").replace("FIRE", "ROLL").strip()

        r.sendlineafter(b'What do you do?', payload.encode())
        tries = tries + 1
        log.info(f'{tries}: {payload}')
    except EOFError:
        log.success(got.strip())
        r.close()
        break 
```

### References
https://infosecwriteups.com/htb-cyber-apocalypse-ctf-2024-misc-9d3d512900b4
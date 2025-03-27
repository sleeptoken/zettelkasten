
2024-08-02 17:05

Source: #htb 

Tags: [[pwntools]] [[Binary Exploit]]

we are given a `docker` `ip` where a command line game is hosted that asks us to replace an occurring of a word w another word eg. if the question says `GORGE` the answer is `STOP`.

this goes on for about 500 more times so we automate it using python

```python
from pwn import *

r = remote('94.237.59.63',46361)

r.recvuntil(b'(y/n) ')

r.sendline(b'y')

r.recvuntil(b'\n')

tries = 0

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

HTB Apocalypse '24 -  stop drop roll
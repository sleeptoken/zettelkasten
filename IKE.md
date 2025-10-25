
2025-10-25 18:40

Source: #htb #Boot2root 

Tags: 
### Enumeration 

rustscan only shows port 22 open, When TCP fails, we turn to its connectionless counterpart, UDP.
```
udpx -t 10.129.13.112 -c 128 -w 1000 
```
- `-c`: Concurrency level
- `-w`: Wait time in milliseconds







### References
https://app.hackthebox.com/machines/736

https://www.isdadev.at/posts/htb-expressway-writeup/
https://thecybersecguru.com/ctf-walkthroughs/mastering-expressway-beginners-guide-from-hackthebox/

IKE Enum - https://routezero.security/2025/04/06/ike-cheat-sheet-for-pentration-testers/
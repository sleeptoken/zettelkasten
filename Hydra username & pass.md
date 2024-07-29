
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

after logging in the website we find a ssh rsa private key, wget it from the website  
The special John the Ripper tool called **ssh2john.py** which can extract the crackable hash from the RSA private key. (don't save o/p in a text file)
```
 /usr/share/john/ssh2john.py id_rsa  > rsa.hash
```
then user rockyou wordlist with john to crack the pass 
chmod the original id_rsa file - `chmod 600 id_rsa`
use ssh to login as john
#### Priv esc

`sudo -l` -> can run `cat` with sudo
go to gtfobins and find exp. for `cat `
since you can cat files using sudo you `cat /etc/shadow` and get a hash of the root pass which you further crack using john 
`sudo cat` the `root/root.txt` and get the flag
### References

[TryHackMe | Brute It](https://tryhackme.com/r/room/bruteit)
[TryHackMe: Brute It - Eric Logan](https://eric.cc/tryhackme-brute-it/)
[TryHackMe | Brute It Walkthrough | Medium](https://enescayvarli.medium.com/tryhackme-bruteit-walkthrough-e93e3f29c233)

Linpeas - [linpeas | grimbins](https://grimbins.github.io/grimbins/linpeas/)
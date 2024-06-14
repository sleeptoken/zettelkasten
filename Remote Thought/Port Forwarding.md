
2024-06-14 10:55

Source: #privesc 

Tags: [[linux]]

For some reason we were not able to run the [[mysql]] client from the debian VM or we wanted to run some commands against the mysql server from our kali machine  

**Adversary  machine:**

- Determine which port mysql listening on using `netstat -nl`
- Since 3306 is default port for mysql our desired local address is shown as `127.0.0.1:3306`
- Note that it is bound to the localhost address meaning we can't access it externally we can fix this by using [[ssh]] port forwarding .
```
ssh -R 4444:127.0.0.1:3306 root@<kali-ip>

4444 on my kali machine 
<kali-ip> is our kali machines ip
```

**Kali machine :**

Here run mysql command and instruct it to connect to port 4444 of my local kali using the root user 
```
mysql -u root -h 127.0.0.1 -P 4444
```
traffic is then forwarded over the ssh connection, this allows you to connect to the debian mysql server remotely 

to check the hostname in mysql use 
`select @@hostname;`


### References
[[Privilege Escalation]]
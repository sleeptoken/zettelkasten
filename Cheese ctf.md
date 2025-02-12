
2025-02-05 21:12

Source: #Boot2root 
### Recon

on running an Nmap scan we see a lot of ports open. The author maybe using portspoofing to show every port as open to attackers 

on visiting the IP we find a website and it has a login page 

> Try to open images in new tabs and see if directory listing works - try going to a upper directory 
> sometimes directory listing reveals sensitive information, including version disclosure [[Bug Bounty]]
### username enum

you can run burp intruder and try brute forcing the username using this wordlists [[and we find that we get a `302` code for a 
particular username 
we can login with that username and a random password 



### References
[TryHackMe | Cyber Security Training](https://tryhackme.com/room/cheesectfv10)

#### walkthrough 
[Cheese CTF TryHackMe Walk-through | by Nawaz | Medium](https://medium.com/@nonawaz/tryhackme-cheese-ctf-walkthrough-73c555d94f4a)
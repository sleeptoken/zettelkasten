
2025-02-05 21:12

Source: #Boot2root 
### Recon

on running an Nmap scan we see a lot of ports open. The author maybe using portspoofing to show every port as open to attackers 

on visiting the IP we find a website and it has a login page 

> Try to open images in new tabs and see if directory listing works - try going to a upper directory 
> sometimes directory listing reveals sensitive information, including version disclosure [[Bug Bounty]]
### Username enum

- you can run burp intruder and try brute forcing the username using this wordlists [[SQL injection]] and we find that we get a `302` code for a particular username 
- we can login with that username and a random password 

we could have also used dirsearch and it would have showed us the hidden `messages.html` directly 

the url reads -> `cheese.thm/secret-script/php?file=php://filter/resource=/etc/passwd`
- appending `/etc/passwd` does confirm directory traversal 
### LFI 2 RCE - PHP Filter 

since the URL shows php filter in it, we can try PHP filter chain attack using the [python tool](https://github.com/synacktiv/php_filter_chain_generator)
follow the commands in [PHP Filters Chain | Exploit Notes](https://exploit-notes.hdks.org/exploit/web/security-risk/php-filters-chain/) -  in order for these to work open a web server `python3 -m http.server 80`
### www-data to user 




### References
[TryHackMe | Cyber Security Training](https://tryhackme.com/room/cheesectfv10)

#### walkthrough 
[Cheese CTF TryHackMe Walk-through | by Nawaz | Medium](https://medium.com/@nonawaz/tryhackme-cheese-ctf-walkthrough-73c555d94f4a)
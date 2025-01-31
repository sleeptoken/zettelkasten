
2024-11-03 22:09

Source: #Boot2root 
### Recon

- Nmap scan reveals port 22, 3000 
- 3000 mei website hai  
- We find `users` username and password in source code of the landing page at 3000
- on logging in as user we get no leads
### Foothold

to login as admin logout and click on `forgot_password`
1. the `forgot_password` page runs a Js script that checks if username is admin or not  
2. Disable [[JavaScript]] and then input admin in username and click `forgot_password`
3. after changing the password, login as admin 
### Exploit

- Admin portal has input field that exposes files 
- Get hold of `id_rsa` - remember to do proper directory traversal
- make sure to give proper permissions  to id_rsa - `chmod 600 id_rsa`
- Login as user(hacker) from the [[ssh (22 port)]] key we got from  
### Privilege Escalation

- Then run linpeas  
- We find a conventions.txt  
- That tells us how passwords are written for different users  
- Create a wordlist as `root<random_6_digit_number>toor`  
- Use hydra to brute force the random 6 digit number  
- And get password of root
### References

DJ Sanghvi CTF breachbytes2
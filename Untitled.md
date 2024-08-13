
2024-08-13 16:34

Source: #CTF #htb 

Tags: [[Linux]]
### Recon

[[nmap]] scan tells us we have 5 ports open 
```
nmap -sS -p- -n -Pn -A --min-rate=9362 $IP
```
- `sS`: This option specifies a `SYN` scan  
- `-p-`: This option tells `nmap` to scan all `65,535` ports.  
- `-n`: This option is speeds up the scan by avoiding `DNS` resolution.  
- `-Pn`: This option disables host discovery, treating all hosts as online. This is useful for scanning hosts that may be blocking `ICMP` ping requests.  
- `--min-rate=9362`: This option sets the minimum number of packets per second to be sent during the scan. This speeds up the scan but may increase the likelihood of being detected by security systems.   
- `-A`: This option enables OS detection, version detection, script scanning, and traceroute.
```
http-title: Did not follow redirect to [http://greenhorn.htb/](http://greenhorn.htb/)
```
indicates that the HTTP service running on port 80 is configured to redirect requests to `[http://greenhorn.htb](http://greenhorn.htb/)` . However `nmap` did not follow this redirect because it doesn't resolve the `greenhorn.htb` domain name

Add an entry to your `/etc/hosts` file to map the `greenhorn.htb` domain to the IP address of the target machine

The URL parameter `file` might be used to include files on the server-side. This can potentially be exploited for Local File Inclusion (LFI) if the parameter is not properly sanitized. but this doesn't work

### Enum

original website served at `http://greenhorn.htb` doesn't give any hints therefore we try connecting to a different port `http://greenhorn.htb:3000` here the website served has a repo containing files of the website at the original address

on searching around a little we find that we have a password file in the `data/settings` directory. we check `data` cause `robots.txt` says not to.
we also find a `login.php` file, that doesn't take a username and the password is what we found hashed in the repo before
### Pluck CMS exploit

on logging in we find the version number, we find a RCE exploit for this on exploitdb
the exploit says that we can get revshell by uploading a zip file in the install module page of the application 
download a `php` revshell from `pentestmonkey` and zip it using 
```shell
zip phprevshell.zip phprevshell.php
```

after getting a shell we see there is a directory name junior in the home directory that we can't access thereby suggesting that there might be a user named junior.
`su junior` to login as junior use the hashed password we found before. 
> **Remember to try out previously found passwords to login** 

along with the user.txt we also have a pdf that we can't open on that machine so we need to transfer that file to our system 
#### Method 1

on pwnbox - 
```
python3 -m http.server 3333
```
on local kali
```
wget greenhorn.htb:3333/openvas.pdf
```

#### Method 2



the pdf contains some instructions to login as root but the password is blurred, it is blurred image of the password pasted on top of the pdf 
to extract image form the pdf use 
##### Method 1


to clean the blurred image use `Depix` tool
```
python3 depix.py \
    -p /path/to/your/input/image.png \
    -s images/searchimages/debruinseq_notepad_Windows10_closeAndSpaced.png \
    -o /path/to/your/output.png
```
after a while we get the password of the root user

### References
https://app.hackthebox.com/machines/GreenHorn

[GitHub - spipm/Depix: Recovers passwords from pixelized screenshots](https://github.com/spipm/Depix)
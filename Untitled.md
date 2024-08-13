
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

### Enum

original website served at `http://greenhorn.htb` doesn't give any hints therefore we try connecting to a different port `http://greenhorn.htb:3000` here the website served has a repo containing files of the website at the original address

on searching around a little we find that we have a password file in the `data/settings` directory. we check `data` cause `robots.txt` says not to.
we also find a `login.php` file, that doesn't take a username and the password is what we found hashed in the repo before
### Pluck CMS exploit

on logging in we find the version number, we find a RCE exploit for this on exploitdb
the exploit says that we can get revshell by 










### References
https://app.hackthebox.com/machines/GreenHorn
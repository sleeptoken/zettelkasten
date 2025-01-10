
2025-01-10 18:34

Source: #CEHv13 

| Passive                                                                            | Active                                                                                               |
| ---------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------- |
| Involves gathering information about the target without direct interaction         | Involves gathering information about the target with direct interaction                              |
| Involes - OSINT, Sharing intelligence and partner organizations or industry groups | Involes - DNS Interrogation, social engineering, network/port scanning, user and service enumeration |
### Footprinting Techniques

![[Screenshot 2025-01-10 190743.png]]

### Google Dorking 

	 Google Hacking Database
once a vulnerable site is identified, attackers attempt to launch various possible attacks, such as buffer overflow and SQL injection, which compromise information security
#### Dorking with AI

Example prompt for ShellGPT
```
use filetype search operator to obtain pdf files on the target website example.org and store the result in the recon.txt file
```

The following shell command is designed to conduct advanced Google hacking using the "filetype" operator to specifically target PDF files within the eccouncil.org domain. The command then saves the obtained results to a file named "recon1.txt":
```sh
lynx --dump "http://www.google.com/search?q=site:eccouncil.org+filetype:pdf" | grep "http" | cut -d "=" -f2 | grep -o "http[^&]*" > reconl.txt 
```

### References
CEH v13 M2

2024-06-01 18:56

Source: #tryhackme #owasp2021 

Tags: [[web]] [[owasp]] [[py]]

#### Security misconfigs were ranked No. 5 on OWASP Top 10 2021

Security misconfigurations include:
- Poorly configured permissions on cloud services, like S3 buckets.
- Having unnecessary features enabled, like services, pages, accounts or privileges.
- Default accounts with unchanged passwords.
- Error messages that are overly detailed and allow attackers to find out more about the system.
- Not using [HTTP security headers](https://owasp.org/www-project-secure-headers/).

**Debugging Interfaces**
Werkzeug is a vital component in Python-based web applications as it provides an interface for web servers to execute the Python code. Werkzeug includes a debug console that can be accessed either via URL on `/console`

Commands on the console can be executed as normal py commands - 
`import os; print(os.popen("ls -l").read())`

### References
[TryHackMe | OWASP Top 10 - 2021](https://tryhackme.com/r/room/owasptop102021)

2024-06-04 11:34

Source: #web
#### Server-Side Request Forgery was ranked No. 10 on OWASP Top 10 2021

This type of vulnerability occurs when an attacker can coerce a web application into sending requests on their behalf to arbitrary destinations while having control of the contents of the request itself. SSRF vulnerabilities often arise from implementations where our web application needs to use third-party services.

Simply speaking a SSRF is an issue in a website, where you can somehow make a server perform a request for you. If a server is inside an internal network, and from the outside you couldn’t access the internal servers, now you could use the vulnerable webserver and make it perform the request

### References
[TryHackMe | OWASP Top 10 - 2021](https://tryhackme.com/r/room/owasptop102021)
[What is SSRF (Server-side request forgery)? Tutorial & Examples | Web Security Academy](https://portswigger.net/web-security/ssrf)
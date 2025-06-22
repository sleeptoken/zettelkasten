
2024-06-04 11:34

Source: #web #portswigger 
#### was ranked No. 10 on OWASP Top 10 2021

This type of vulnerability occurs when an attacker can coerce a web application into sending requests on their behalf to arbitrary destinations while having control of the contents of the request itself. SSRF vulnerabilities often arise from implementations where our web application needs to use third-party services.

Essentially this allows an attacker to cause the server-side application to make requests to an unintended location. If a server is inside an internal network, and from the outside you couldn’t access the internal servers, now you could use the vulnerable webserver and make it perform the request
#### Impact of SSRF attacks?

- Unauthorized actions or access to data 
- Perform arbitrary command injection
- An SSRF exploit that causes connections to external third-party systems might result in malicious onward attacks
#### Common SSRF attacks

SSRF attacks often exploit trust relationships to escalate an attack from the vulnerable application and perform unauthorized actions. These trust relationships might exist in relation to the server, or in relation to other back-end systems within the same organization.



#### [web/loopy](https://ctf.gg/blog/tjctf-2025/beginner-web)

Classic SSRF chall. Likely you can host a page that redirects to localhost but we just use some domain that has an A record to 127.0.0.1, and fetched `http://<domain>:5000/admin` to get the flag (we conveniently already had one on hand).:

### References
[TryHackMe | OWASP Top 10 - 2021](https://tryhackme.com/r/room/owasptop102021)
[What is SSRF (Server-side request forgery)? Tutorial & Examples | Web Security Academy](https://portswigger.net/web-security/ssrf)
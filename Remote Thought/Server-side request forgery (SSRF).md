
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

#### SSRF attacks against the server 

In an SSRF attack against the server, the attacker causes the application to make an HTTP request back to the server that is hosting the application, via its loopback network interface. This typically involves supplying a URL with a hostname like `127.0.0.1` (a reserved IP address that points to the loopback adapter) or localhost (a commonly used name for the same adapter).

#### Why Trust Relations exists 

- The access control check might be implemented in a different component that sits in front of the application server. When a connection is made back to the server, the check is bypassed.
- For disaster recovery purposes, the application might allow administrative access without logging in, to any user coming from the local machine. This provides a way for an administrator to recover the system if they lose their credentials. This assumes that only a fully trusted user would come directly from the server.
- The administrative interface might listen on a different port number to the main application, and might not be reachable directly by users.

#### SSRF attacks against other back-end systems

In some cases, the application server is able to interact with back-end systems that are not directly reachable by users. 
- These systems often have non-routable private IP addresses. 
- The back-end systems are normally protected by the network topology, so they often have a weaker security posture. In many cases, internal back-end systems contain sensitive functionality that can be accessed without authentication by anyone who is able to interact with the systems.

can try brute forcing IP address or port to the non routable private IP address.

#### Circumventing common SSRF defenses
##### SSRF with blacklist-based input filters

Some applications block input containing hostnames like 127.0.0.1 and localhost, or sensitive URLs like /admin. In this situation, you can often circumvent the filter using the following techniques:

- Use an alternative IP representation of 127.0.0.1, such as 2130706433, 017700000001, or 127.1.
- Register your own domain name that resolves to 127.0.0.1. You can use `spoofed.burpcollaborator.net` for this purpose.
- Obfuscate blocked strings using URL encoding or case variation.
- Provide a URL that you control, which redirects to the target URL. Try using different redirect codes, as well as different protocols for the target URL. For example, switching from an `http:` to `https:` URL during the redirect has been shown to bypass some anti-SSRF filters.

When double coding use the burp extension - hackvertor 
- select the text then right click > extension > hackvetor > encode > `url_encode_all` > select the text incl. tags > extension > hackvetor > convert tags .


#### [web/loopy](https://ctf.gg/blog/tjctf-2025/beginner-web)

Classic SSRF chall. Likely you can host a page that redirects to localhost but we just use some domain that has an A record to 127.0.0.1, and fetched `http://<domain>:5000/admin` to get the flag (we conveniently already had one on hand).:

### References
[TryHackMe | OWASP Top 10 - 2021](https://tryhackme.com/r/room/owasptop102021)
[What is SSRF (Server-side request forgery)? Tutorial & Examples | Web Security Academy](https://portswigger.net/web-security/ssrf)
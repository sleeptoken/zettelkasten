
2024-06-05 11:59

Source: #tryhackme 

Tags: [[web]] [[Injection]] 

XSS is a vulnerability that allows an attacker to inject malicious scripts into a web page viewed by another user. Consequently, they bypass the **Same-Origin Policy (SOP)**; SOP is a security mechanism implemented in modern web browsers to prevent a malicious script on one web page from obtaining access to sensitive data on another page. SOP defines origin based on the protocol, hostname, and port. Consequently, a malicious ad cannot access data or manipulate the page or its functionality on another origin, such as an online shop or bank page. XSS dodges SOP as it is executing from the same origin.
- exploits are browser specific

**Encoding**: The [[JavaScript]] function `btoa("string")` encodes a string of binary data to create a base64-encoded ASCII string. This is useful to remove white space and special characters or encode other alphabets. The reverse function is `atob("base64_string")`

Types of XSS- 
- [[Reflected XSS]]
- [[Stored XSS]]
- [[DOM-based XSS]]




### References
[TryHackMe | XSS](https://tryhackme.com/r/room/axss)
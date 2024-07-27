
2024-06-05 11:59

Source: #tryhackme #web

Tags: [[Injection]] 

XSS is a vulnerability that allows an attacker to inject malicious scripts into a web page viewed by another user. Consequently, they bypass the **Same-Origin Policy ([[SOP]])**; SOP is a security mechanism implemented in modern web browsers to prevent a malicious script on one web page from obtaining access to sensitive data on another page. SOP defines origin based on the protocol, hostname, and port. Consequently, a malicious ad cannot access data or manipulate the page or its functionality on another origin, such as an online shop or bank page. XSS dodges SOP as it is executing from the same origin.
- exploits are browser specific

**Encoding**: The [[JavaScript]] function `btoa("string")` encodes a string of binary data to create a base64-encoded ASCII string. This is useful to remove white space and special characters or encode other alphabets. The reverse function is `atob("base64_string")`

Types of XSS- 
- [[Reflected XSS]]
- [[Stored XSS]]
- [[DOM-based XSS]]

- Various repositories can be consulted to build your custom XSS payload.  [Payload List](https://github.com/payloadbox/xss-payload-list)
- sometimes, there are filters blocking XSS payloads. If there is a limitation based on the payload length, then [Tiny Payloads](https://github.com/terjanq/Tiny-XSS-Payloads)
- If XSS payloads are blocked based on specific blocklists, there are various tricks for evasion. For instance, a horizontal tab, a new line, or a carriage return can break up the payload, based on the [XSS Filter Evasion Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/XSS_Filter_Evasion_Cheat_Sheet.html), we can break up the payload.

### References
[TryHackMe | XSS](https://tryhackme.com/r/room/axss)
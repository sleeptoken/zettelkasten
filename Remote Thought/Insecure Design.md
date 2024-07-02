
2024-06-04 10:19

Source: #tryhackme #owasp2021 #web 

Tags: [[owasp]]

#### Insecure Design was ranked No. 4 on OWASP Top 10 2021

**Insecure design** refers to vulnerabilities which are inherent to the application's architecture.

##### Insecure Password Resets

 Instagram allowed users to reset their forgotten passwords by sending them a 6-digit code to their mobile number via SMS for validation. If an attacker wanted to access a victim's account, he could try to brute-force the 6-digit code. As expected, this was not directly possible as Instagram had rate-limiting implemented so that after 250 attempts, the user would be blocked from trying further. However, it was found that the rate-limiting only applied to code attempts made from the same IP. If an attacker had several different IP addresses from where to send requests, he could now try 250 codes per IP. For a 6-digit code, you have a million possible codes, so an attacker would need 1000000/250 = 4000 IPs to cover all possible codes. This may sound like an insane amount of IPs to have, but cloud services make it easy to get them at a relatively small cost, making this attack feasible.



### References
[TryHackMe | OWASP Top 10 - 2021](https://tryhackme.com/r/room/owasptop102021)
[TryHackMe | Cyber Security Training](https://tryhackme.com/r/room/securesdlc)
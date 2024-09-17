
2024-09-17 14:39

Source: #portswigger #web 

Authentication is the process of verifying the identity of a user or client. 
There are three main types of authentication:

   1. Something you `know` -
		   such as a password or the answer to a security question. These are sometimes called "knowledge factors".
   1. Something you `have` -
		   This is a physical object such as a mobile phone or security token. These are sometimes called "possession factors".
   1. Something you `are or do` - 
		   For example, your biometrics or patterns of behavior. These are sometimes called "inherence factors".
### What is the difference between authentication and authorization?

Authentication is the process of verifying that a user is who they claim to be. Authorization involves verifying whether a user is allowed to do something. 
### How do authentication vulnerabilities arise?

Most vulnerabilities in authentication mechanisms occur in one of two ways:

1. The authentication mechanisms are weak because they fail to adequately protect against brute-force attacks.
2. Logic flaws or poor coding in the implementation allow the authentication mechanisms to be bypassed entirely by an attacker. This is sometimes called "broken authentication".



### References
https://portswigger.net/web-security/authentication
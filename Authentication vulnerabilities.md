
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
### Brute-forcing 

#### Usernames

Usernames are especially easy to guess if they conform to a recognizable pattern, such as an email address. For example, it is very common to see business logins in the format `firstname.lastname@somecompany.com`. 
During auditing, check whether the website discloses potential usernames publicly. For example, are you able to access user profiles without logging in? Even if the actual content of the profiles is hidden, the name used in the profile is sometimes the same as the login username. You should also check HTTP responses to see if any email addresses are disclosed. Occasionally, responses contain email addresses of high-privileged users, such as administrators or IT support.
#### Password

However, while high-entropy passwords are difficult for computers alone to crack, we can use a basic knowledge of human behavior to exploit the vulnerabilities that users unwittingly introduce to this system. Rather than creating a strong password with a random combination of characters, users often take a password that they can remember and try to crowbar it into fitting the password policy.
### Username enumeration

While attempting to brute-force a login page, you should pay particular attention to any differences in:
###### Status Codes:
During a brute-force attack, the returned HTTP status code is likely to be the same for the vast majority of guesses because most of them will be wrong. 
If a guess returns a different status code, this is a strong indication that the username was correct. It is best practice for websites to always return the same status code regardless of the outcome, but this practice is not always followed.
###### Error messages: 
Sometimes the returned error message is different depending on whether both the username AND password are incorrect or only the password was incorrect. It is best practice for websites to use identical, generic messages in both cases.
###### Response times:
If most of the requests were handled with a similar response time, any that deviate from this suggest that something different was happening behind the scenes. This is another indication that the guessed username might be correct. For example, a website might only check whether the password is correct if the username is valid. This extra step might cause a slight increase in the response time. 
This may be subtle, but an attacker can make this delay more obvious by entering an excessively long password that the website takes noticeably longer to handle.




### References
https://portswigger.net/web-security/authentication
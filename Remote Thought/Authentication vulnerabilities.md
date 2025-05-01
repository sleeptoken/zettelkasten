
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
###### Status Codes
It is best practice for websites to always return the same status code regardless of the outcome, but this practice is not always followed.
###### Error messages
Sometimes the returned error message is different depending on whether both the username AND password are incorrect or only the password was incorrect. It is best practice for websites to use identical, generic messages in both cases.
###### Response times
For example, a website might only check whether the password is correct if the username is valid. This extra step might cause a slight increase in the response time. 
This may be subtle, but an attacker can make this delay more obvious by entering an excessively long password that the website takes noticeably longer to handle.
#### *Lab* 

in [[burpsuite]] while setting the payload position in intruder 
Click on the Settings tab to open the Settings side panel. Under `Grep - Extract`, click Add. In the dialog that appears, scroll down through the response until you find the error message `Invalid username or password.` Use the mouse to highlight the text content of the message. The other settings will be automatically adjusted. Click `OK` and then start the attack. 

#### *Lab*: Username enumeration via response timing

while brute forcing Experiment with different usernames and passwords. Notice that your IP will be blocked if you make too many invalid login attempts. 
Identify that the `X-Forwarded-For` header is supported, which allows you to spoof your IP address and bypass the IP-based brute-force protection.

- Send this request to Burp Intruder and select Pitchfork attack from the attack type drop-down menu. Add the `X-Forwarded-For` header.
- Add payload positions for the `X-Forwarded-For` header and the username parameter. Set the password to a very long string of characters (about 100 characters should do it).
- In the Payloads side panel, select position 1 from the Payload position drop-down list. Select the Numbers payload type. Enter the range 1 - 100 and set the step to 1. Set the max fraction digits to 0. This will be used to spoof your IP.
- Select position 2 from the Payload position drop-down list, then add the list of usernames. Start the attack.
- When the attack finishes, at the top of the dialog, click Columns and select the Response received and Response completed options. These two columns are now displayed in the results table. Notice that one of the response times was significantly longer than the others

Add the `X-Forwarded-For` header again and add a payload position to it. Insert the username that you just identified and add a payload position to the password parameter, find the response with a 302 status code
### Flawed brute-force protection

The two most common ways of preventing brute-force attacks are:

- Locking the account that the remote user is trying to access if they make too many failed login attempts
- Blocking the remote user's IP address if they make too many login attempts in quick succession

These aren't fail proof. For example, you might sometimes find that your IP is blocked if you fail to log in too many times. In some implementations, the counter for the number of failed attempts resets if the IP owner logs in successfully. This means an attacker would simply have to log in to their own account every few attempts to prevent this limit from ever being reached.
#### *Lab* - Broken brute-force protection, IP block

we find the same condition as mentioned in the example above 
to bypass that we use custom lists -
- username - 2 Carlos , 1 wiener ... and repeat
- password - 2 passwords from the given list , 1 peter ... and repeat 
this ensures that we perform a successful login to reset the counter after every 2 unsuccessful attempts
further go to resource pool and change the max concurrent requests to 1, making sure that 1 request is made at a time and also making sure that the sequence is followed 
hit attack and get the answer 
### Account Locking




### References
https://portswigger.net/web-security/authentication
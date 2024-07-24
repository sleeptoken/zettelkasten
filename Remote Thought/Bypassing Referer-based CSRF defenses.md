
2024-07-24 14:51

Source: #portswigger #web 

Tags: [[CSRF]]

Aside from defenses that employ CSRF tokens, some applications make use of the HTTP Referer header to attempt to defend against CSRF attacks, normally by verifying that the request originated from the application's own domain.

The HTTP Referer header (which is inadvertently misspelled in the HTTP specification) is an optional request header that contains the URL of the web page that linked to the resource that is being requested.

### Validation of Referer depends on header being present

Some applications validate the Referer header when it is present in requests but skip the validation if the header is omitted. 
There are various ways to achieve this, but the easiest is using a META tag within the HTML page that hosts the CSRF attack:
```
<meta name="referrer" content="never">
```

### Validation of Referer can be circumvented

Some applications validate the Referer header in a naive way that can be bypassed. For example, if the application validates that the domain in the Referer starts with the expected value, then the attacker can place this as a subdomain of their own domain:

```
http://vulnerable-website.com.attacker-website.com/csrf-attack
```

Likewise, if the application simply validates that the Referer contains its own domain name, then the attacker can place the required value elsewhere in the URL:

```
http://attacker-website.com/csrf-attack?vulnerable-website.com
```

Although you may be able to identify this behavior using Burp, you will often find that this approach no longer works when you go to test your proof-of-concept in a browser. In an attempt to reduce the risk of sensitive data being leaked in this way, many browsers now strip the query string from the Referer header by default.

You can override this behavior by making sure that the response containing your exploit has the `Referrer-Policy: unsafe-url` header set (note that Referrer is spelled correctly in this case, just to make sure you're paying attention!). This ensures that the full URL will be sent, including the query string.

##### Lab :
 
This lab's email change functionality is vulnerable to CSRF. It attempts to detect and block cross domain requests, but the detection mechanism can be bypassed. 

sometimes what applications do is they don't validate that this entire referrer header is equal to the domain of the application and instead just validate that the domain of the application is contained in the referrer header 

when creating the CSRF POC use [[JavaScript]]
```
history.pushState("", "", "/?YOUR-LAB-ID.web-security-academy.net")
```
- history.pushState what this method does is it adds an entry to the browser session history stack 
- It has three parameters
	- the first one is the state object 
	- the second one is the title 
	- the third one is the url 

If you store the exploit and test it, you may encounter the "invalid Referer header" error again. This is because many browsers now strip the query string from the Referer header by default as a security measure to bypass this use - 
```
Referrer-Policy: unsafe-url
```

### Testing CSRF via Referer Header

1. Remove the referer header
2. check which portion of the referer header is the application validating

### References

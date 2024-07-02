
2024-06-19 21:18

Source: #portswigger #web

Tags:  [[API]]

- Clickjacking is an interface-based attack in which a user is tricked into clicking on actionable content on a hidden website by clicking on some other content in a decoy website.
- This attack **differs from a [[CSRF]]** attack in that the user is required to perform an action such as a button click whereas a CSRF attack depends upon forging an entire request without the user's knowledge or input. 

Protection against CSRF attacks is often provided by the use of a CSRF token: a session-specific, single-use number or nonce. Clickjacking attacks are not mitigated by the CSRF token as a target session is established with content loaded from an authentic website and with all requests happening on-domain. CSRF tokens are placed into requests and passed to the server as part of a normally behaved session. The difference compared to a normal user session is that the process occurs within a hidden `iframe`. 

in clickjaking you got to place an `iframe` in another website and align the buttons in the `iframe` with the buttons on the other website using `css` and reduce transparency of the `iframe` to 0.00001 so that the victim couldn't make out. 
#### Frame busting scripts

 - Clickjacking attacks are possible whenever websites can be framed. Therefore, preventative techniques are based upon restricting the framing capability for websites.
 - A common client-side protection enacted through the web browser is to use frame busting or frame breaking scripts. These can be implemented via proprietary browser JavaScript add-ons or extensions such as NoScript. 
 - As frame busters are JavaScript then the browser's security settings may prevent their operation or indeed the browser might not even support JavaScript.
 - An effective attacker workaround against [[frame busters]] is to use the HTML5 `iframe` `sandbox` attribute. When this is set with the `allow-forms` or `allow-scripts` values and the `allow-top-navigation` value is omitted then the frame buster script can be neutralized as the iframe cannot check whether or not it is the top window: 
 
The XSS exploit is combined with the iframe target URL so that the user clicks on the button or link and consequently executes the DOM XSS attack. 

#### Clickjacking with a [[DOM-based XSS]] attack 

if there is a form on a website and we want the form to be prepopulated with custom values then go to inspect element and see the names of the form fields and create a custom `GET` request from the `URL`

 Two mechanisms for server-side clickjacking protection are `X-Frame-Options` and `Content Security Policy`. 

#### X-Frame-Options - [[API Testing]]

The header provides the website owner with control over the use of `iframes` or objects so that inclusion of a web page within a frame can be prohibited with the deny directive:
```
X-Frame-Options: deny
```

Alternatively, framing can be restricted to the same origin as the website using the `sameorigin` directive
```
X-Frame-Options: sameorigin
```

or to a named website using the `allow-from` directive:
```
X-Frame-Options: allow-from https://normal-website.com
```

#### Content Security Policy (CSP) - [[API Testing]]

CSP is usually implemented in the web server as a return header of the form:
```
Content-Security-Policy: policy
```
where policy is a string of policy directives separated by semicolons. 

 The recommended clickjacking protection is to incorporate the `frame-ancestors` directive in the application's Content Security Policy. 
 - The `frame-ancestors 'none'` directive is similar in behavior to the `X-Frame-Options` `deny` directive. 
 - The `frame-ancestors 'self'` directive is broadly equivalent to the `X-Frame-Options` `sameorigin` directive. 
 
The following CSP whitelists frames to the same domain only:
```
Content-Security-Policy: frame-ancestors 'self';
```

Alternatively, framing can be restricted to named sites:
```
Content-Security-Policy: frame-ancestors normal-website.com;
```
### References

https://portswigger.net/web-security/learning-paths/clickjacking
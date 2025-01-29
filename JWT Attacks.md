
2025-01-24 11:59

Source: #portswigger 

Tags: [[JSON web tokens]]

## Exploiting flawed JWT signature verification

By design, servers don't usually store any information about the JWTs that they issue. Instead, each token is an entirely self-contained entity. This has several advantages, but also introduces a fundamental problem - the server doesn't actually know anything about the original contents of the token, or even what the original signature was. Therefore, if the server doesn't verify the signature properly, there's nothing to stop an attacker from making arbitrary changes to the rest of the token.
#### Accepting arbitrary signatures

JWT libraries typically provide one method for verifying tokens and another that just decodes them. For example, the Node.js library `jsonwebtoken` has `verify()` and `decode()`. Devs usually ignore the `verify()` method. This effectively means that the application doesn't verify the signature at all.




### References
[JWT attacks | Web Security Academy](https://portswigger.net/web-security/jwt)
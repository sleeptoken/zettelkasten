
2025-01-24 11:59

Source: #portswigger 

Tags: [[JSON web tokens]]

## Exploiting flawed JWT signature verification

By design, servers don't usually store any information about the JWTs that they issue. Instead, each token is an entirely self-contained entity. This has several advantages, but also introduces a fundamental problem - the server doesn't actually know anything about the original contents of the token, or even what the original signature was. Therefore, if the server doesn't verify the signature properly, there's nothing to stop an attacker from making arbitrary changes to the rest of the token.
### Accepting arbitrary signatures

JWT libraries typically provide one method for verifying tokens and another that just decodes them. For example, the Node.js library `jsonwebtoken` has `verify()` and `decode()`. Devs usually ignore the `verify()` method. This effectively means that the application doesn't verify the signature at all.
#### Lab 
JWT authentication bypass via unverified signature

- Intercept the request to manipulate the jwt token, change the URL to admin and change the payload of jwt from user wiener to administrator. 
- The response show the API to delete username Carlos
- In the same request change the username form `admin` to `admin/delete?username=carlos` and remember to change the jwt token to suit that of the administrator  
### Accepting tokens with no signature

`alg` parameter in the JWT header is inherently flawed because the server has no option but to implicitly trust user-controllable input from the token which, at this point, hasn't been verified at all. In other words, an attacker can directly influence how the server checks whether the token is trustworthy.

JWTs can be signed using a range of different algorithms, but can also be left unsigned. In this case, the `alg` parameter is set to `none` [[API Testing]], which indicates a so-called "unsecured JWT". 
Due to the obvious dangers of this, servers usually reject tokens with no signature. However, as this kind of filtering relies on string parsing, you can sometimes bypass these filters using classic obfuscation techniques, such as mixed capitalization and unexpected encodings.
#### Lab

Follow the same method as in the previous lab but this time change the `alg` parameter from the header to `none`, change the username to `administrator`. In the message editor, remove the signature from the JWT, but remember to leave the trailing dot after the payload.
## Brute-forcing secret keys

Some signing algorithms, such as HS256 (HMAC + SHA-256), use an arbitrary, standalone string as the secret key. Just like a password, Attackers may be able to create JWTs with any header and payload values they like, then use the key to re-sign the token with a valid signature.
#### Brute-forcing secret keys using hashcat

You just need a valid, signed JWT from the target server and a wordlist of well-known secrets. You can then run the following command, passing in the JWT and wordlist as arguments:

```
hashcat -a 0 -m 16500 <jwt> <wordlist>
```

[[Hashcat]] signs the header and payload from the JWT using each secret in the wordlist, then compares the resulting signature with the original one from the server.


### References
[JWT attacks | Web Security Academy](https://portswigger.net/web-security/jwt)

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

you can change the JWT token using [JSON Web Tokens - jwt.io](https://jwt.io/)
or use burp extension - JWT editor
## JWT header parameter injections

According to the JWS specification, only the `alg` header parameter is mandatory. In practice, however, JWT headers (also known as JOSE headers) often contain several other parameters. The following ones are of particular interest to attackers.

- `jwk` (JSON Web Key) - Provides an embedded JSON object representing the key.
    
- `jku` (JSON Web Key Set URL) - Provides a URL from which servers can fetch a set of keys containing the correct key.
    
- `kid` (Key ID) - Provides an ID that servers can use to identify the correct key in cases where there are multiple keys to choose from. Depending on the format of the key, this may have a matching `kid` parameter.
### Injecting self-signed JWTs via the jwk parameter

`jwk` header parameter, which servers can use to embed their public key directly within the token itself in JWK format.

Ideally, servers should only use a limited whitelist of public keys to verify JWT signatures. However, misconfigured servers sometimes use any key that's embedded in the `jwk` parameter.

You can also perform this attack manually by adding the `jwk` header yourself. However, you may also need to update the JWT's `kid` header parameter to match the `kid` of the embedded key. The extension's built-in attack takes care of this step for you.
#### Lab

The captured JWT of a normal user has a `kid` parameter and the `alg : RS256` parameter 
1. Go to the JWT editor tab and click `new RSA key`, directly generate and click `OK` to save the key
2. Go back to the `GET /admin` request in Burp Repeater and switch to the extension-generated `JSON Web Token` tab.
3. In the payload, change the value of the `sub` claim to `administrator`.
4. At the bottom of the **JSON Web Token** tab, click **Attack**, then select **Embedded JWK**. When prompted, select your newly generated RSA key and click **OK**.
5. In the header of the JWT, observe that a `jwk` parameter has been added containing your public key.
### Injecting self-signed JWTs via the jku parameter

Instead of embedding public keys directly using the `jwk` header parameter, some servers let you use the `jku` (JWK Set URL) header parameter to reference a JWK Set containing the key. When verifying the signature, the server fetches the relevant key from this URL.

A JWK Set is a JSON object containing an array of JWKs representing different keys.
JWK Sets like this are sometimes exposed publicly via a standard endpoint, such as `/.well-known/jwks.json`
#### Lab

Here we have a exploit server here, change the Content-Type: 




### References
[JWT attacks | Web Security Academy](https://portswigger.net/web-security/jwt)
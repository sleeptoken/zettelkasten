
2024-06-04 11:08

Source: 

Tags: 

JWTs are very simple tokens that allow you to store key-value pairs on a token that provides integrity as part of the token

![[Pasted image 20240604110852.png]]

- The header contains metadata indicating this is a JWT, and the signing algorithm in use is HS256.
- The payload contains the key-value pairs with the data that the web application wants the client to store.
- The signature is similar to a hash, taken to verify the payload's integrity.

If you change the payload, the web application can verify that the signature won't match the payload and know that you tampered with the JWT. Unlike a simple hash, this signature involves the use of a secret key held by the server only, which means that if you change the payload, you won't be able to generate the matching signature unless you know the secret key.

Notice that each of the 3 parts of the token is simply plaintext encoded with base64



### References

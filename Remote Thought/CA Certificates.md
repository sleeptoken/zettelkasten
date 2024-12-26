
2024-12-26 10:54

Source: #AOC24 #web 
### Certificate

- **Public key**: At its core, a certificate contains a public key, part of a pair of cryptographic keys: a public key and a private key. The public key is made available to anyone and is used to encrypt data.
- **Private key**: The private key remains secret and is used by the website or server to decrypt the data.
- **Metadata**: Along with the key, it includes metadata that provides additional information about the certificate holder (the website) and the certificate.
### Certificate Authority (CA)?
A CA is a trusted entity that issues certificates

Here is a breakdown of what happens with a certificate:
- **Handshake**: Your browser requests a secure connection, and the website responds by sending a certificate, but in this case, it only requires the public key and metadata.
- **Verification:** Your browser checks the certificate for its validity by checking if it was issued by a trusted CA. If the certificate hasn’t expired or been tampered with, and the CA is trusted, then the browser gives the green light. There are different types of checks you can do; check them [here](https://www.sectigo.com/resource-library/dv-ov-ev-ssl-certificates).
- **Key exchange**: The browser uses the public key to encrypt a session key, which encrypts all communications between the browser and the website.
- **Decryption**: The website (server) uses its private key (Never leaves the server) to decrypt the session key, which is [symmetric](https://deviceauthority.com/symmetric-encryption-vs-asymmetric-encryption/). Now that both the browser and the website share a secret key (session key), we have established a secure and encrypted communication!
#### Self-Signed Certificates vs. Trusted CA Certificates

The process of acquiring a certificate with a CA is long, you create the certificate, and send it to a CA to sign it for you. If you don’t have tools and automation in place, this process can take weeks. Self-signed certificates are signed by an entity usually the same one that authenticates. For example, Wareville owns the GiftScheduler site, and if they create a certificate and sign it with Wareville as a CA, that becomes a self-signed certificate.

- **Browsers** generally do not trust self-signed certificates because there is no third-party verification. The browser has no way of knowing if the certificate is authentic or if it’s being used for malicious purposes (like a **man-in-the-middle attack**).
- **Trusted CA certificates**, on the other hand, are verified by a CA, which acts as a trusted third party to confirm the website’s identity.

If you want to test a development environment, it can make sense to use self-signed certificates. Ideally, this is an internal, air-gapped environment with no connection to the public Internet. Otherwise, it defeats the purpose of a certificate: the entire system of secure communication relies on the fact that both parties (the browser and the server) can trust the data being exchanged and that no one in the middle can intercept or modify it without detection.
### Certificate Transparency 



### References
Day 14
[Certificate Transparency : Certificate Transparency](https://certificate.transparency.dev/)
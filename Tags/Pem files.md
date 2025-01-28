
2025-01-24 10:34

PEM is a container file format often used to store cryptographic keys.
- It simply defines the structure and encoding type of the file used to store a bit of data.

PEM is just a standard; they contain text, and the format dictates that PEM files start with...
```
-----BEGIN <type>-----
```

...and end with:

```
-----END <type>-----
```
### PEM Files with SSL Certificates

PEM files are used to store SSL certificates and their associated private keys. Multiple certificates are in the full SSL chain, and they work in this order:

- The end-user certificate, which is assigned to your domain name by a certificate authority (CA). This is the file you use in nginx and Apache to encrypt HTTPS.
- Up to four optional intermediate certificates, given to smaller certificate authorities by higher authorities.
- The root certificate, the highest certificate on the chain, which is self-signed by the primary [[CA Certificates]]










These files are used for TLS/SSL encryption over the web so we can use `openssl` tool to analyze the `"*.pem"` file

use the below command to view the contents of the file.
```sh
openssl x509 -in file.pem -text
```
`x509` option is used for displaying and manipulating X.509 Certifications, which are part of SSL/TLS security.

### References
[What Is a PEM File and How Do You Use It?](https://www.howtogeek.com/devops/what-is-a-pem-file-and-how-do-you-use-it/)
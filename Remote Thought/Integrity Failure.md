
2024-06-04 10:38

Source: #tryhackme #owasp2021 #web 

Tags:  [[owasp]] [[hashing]] [[JWT]]


#### Integrity Failure was ranked No.8 on OWASP Top 10 2021

When talking about integrity, we refer to the capacity we have to ascertain that a piece of data remains unmodified .We use Hash to check if the file was tampered in transit 

To calculate the different hashes in Linux - 
`md5sum file.name`
`sha1sum file.name`
`sha256sum file.name`
#### Software Integrity Failures

 a website that uses third-party libraries that are stored in some external servers that are out of your control. Take as an example jQuery. You can include jQuery in your website directly from their servers without actually downloading it
 
Modern browsers allow you to specify a hash along the library's URL so that the library code is executed only if the hash of the downloaded file matches the expected value. This security mechanism is called Subresource Integrity (SRI), and you can read more about it [here](https://www.srihash.org/).
#### ﻿Data Integrity Failures﻿

JWT is one of the integrity mechanisms to guarantee that the cookie hasn't been altered by the user

A data integrity failure vulnerability was present on some libraries implementing JWTs a while ago. The vulnerable libraries allowed attackers to bypass the signature validation by changing the two following things in a JWT:

1. Modify the header section of the token so that the `alg` header would contain the value `none`.
2. Remove the signature part.


### References
[TryHackMe | OWASP Top 10 - 2021](https://tryhackme.com/r/room/owasptop102021)
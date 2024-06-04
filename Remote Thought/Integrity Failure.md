
2024-06-04 10:38

Source: #tryhackme #owasp2021 

Tags: [[web]] [[owasp]] [[hashing]] [[JWT]]


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
JWTs are very simple tokens that allow you to store key-value pairs on a token that provides integrity as part of the token


### References
[TryHackMe | OWASP Top 10 - 2021](https://tryhackme.com/r/room/owasptop102021)

2024-05-31 13:12

Source: #portswigger

Tags: [[web]]

if the ../../ sequence is blocked then try using the desired directory path directly sometimes the app is misconfigured                 
example. `/image?filename=/etc/passwd` 


**Nested traversal sequences**, such as `....// or ....\/`
this can be used when simple path traversal sequences doesn't lead to actual path traversal ,it just removes all occurrences of `../` from the filename
To obtain a result of `../../../etc/passwd`
I request  `....//....//....//etc/passwd`

**Encoded Path Traversal** 
web servers may strip any directory traversal sequences before passing your input to the application .U can sometimes bypass this kind of sanitization by URL encoding, or even double URL encoding, the `../` characters. Various non-standard encodings, such as `..%c0%af` or `..%ef%bc%8f`, may also work

### References
https://portswigger.net/web-security/learning-paths/path-traversal
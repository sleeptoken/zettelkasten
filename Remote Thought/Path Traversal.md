
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

An application may require the user-supplied filename to end with an expected file extension, such as .png. In this case, it might be possible to use a null byte to effectively terminate the file path before the required extension. For example: filename=`../../../etc/passwd%00.png`

#### Prevention 
to avoid passing user-supplied input to filesystem APIs altogether
If you can't avoid passing user-supplied input to filesystem APIs, then -
Compare the user input with a whitelist of permitted values
verify that the input contains only permitted content, such as alphanumeric characters only. 

### References
https://portswigger.net/web-security/learning-paths/path-traversal
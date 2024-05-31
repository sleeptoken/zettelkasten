
2024-05-31 13:12

Source: #portswigger

Tags: [[web]]

if the ../../ sequence is blocked then try using the desired directory path directly sometimes the app is misconfigured                   example. `/image?filename=/etc/passwd` 

 nested traversal sequences, such as `....// or ....\/`
this can be used when simple path traversal sequences doesn't lead to actual path traversal ,it just removes all occurrences of `../` from the filename
To obtain a result of `../../../etc/passwd`
I request  `....//....//....//etc/passwd`

### References
https://portswigger.net/web-security/learning-paths/path-traversal
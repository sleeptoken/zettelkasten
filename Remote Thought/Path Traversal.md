
2024-05-31 13:12

Source: #portswigger

Tags: 

if the ../../ sequence is blocked then try using the desired directory path directly sometimes the app is misconfigured                   example. `/image?filename=/etc/passwd` 

 nested traversal sequences, such as `....// or ....\/`


### References
https://portswigger.net/web-security/learning-paths/path-traversal

2025-01-24 10:34

Source: 

These files are used for TLS/SSL encryption over the web so we can use `openssl` tool to analyze the `"*.pem"` file

use the below command to view the contents of the file.
```sh
openssl x509 -in file.pem -text
```
`x509` option is used for displaying and manipulating X.509 Certifications, which are part of SSL/TLS security.

### References
[What Is a PEM File and How Do You Use It?](https://www.howtogeek.com/devops/what-is-a-pem-file-and-how-do-you-use-it/)
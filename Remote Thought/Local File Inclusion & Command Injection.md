
2024-07-04 12:47

Source: #web #overthewire #natas

Tags: [[LFI]]

#### Natas 3

always take a look at `robots.txt` 
#### Natas 4 
```
access disallowed. You are visiting from "natas4" while authorized users should come only from "natas5"
```

change the referrer in burp site and we will get the answer 
#### Natas 5

```
you are not logged in
```

change the loggedin parameter from 0 to 1
#### Natas 7

Local file inclusion (hint in the source code)
add the desired directory (`/etc/natas_webpass/natas8` in our case) after the page parameter
#### Natas 9 

you can use `;` to separate the request 
in the parameter which takes user input eg.` needle=; cat /etc/pass`
#### Natas 10 

if proper sanitization is done and the `;` is not working then try `url encoding 


### References
command injection - https://owasp.org/www-community/attacks/Command_Injection

LFI - https://owasp.org/www-project-web-security-testing-guide/v42/4-Web_Application_Security_Testing/07-Input_Validation_Testing/11.1-Testing_for_Local_File_Inclusion

cheatsheet OS command injection - https://cheatsheetseries.owasp.org/cheatsheets/OS_Command_Injection_Defense_Cheat_Sheet.html

Command injection payload list - https://github.com/payloadbox/command-injection-payload-list

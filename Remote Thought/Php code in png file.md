
2025-04-10 11:39

Source: #reversing 

Tags: [[png]] 

We get a html file. So we look at page source of _home.html_. One line in particular stands out:

```
<?php include "img/flare-on.png" ?>
```

Why would a PNG image be loaded as a PHP script? When we open this image with an image viewer, the banner comes up so it is definitely an image. Since we know that the image is being loaded as a PHP script, we search for _php_ inside of the image file , Use `HxD`

Strings equivalent in windows is 
```
more flare-on.png | findstr "."
```


### References
Flare-on 2014 C2
https://cloud.google.com/blog/topics/threat-intelligence/the-flare-on-challen/
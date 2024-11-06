
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

### Dynamic test setup

We are going to create a dynamic test setup to simulate a machine in the middle attacker in order to exploit the pocket text map.

head over to Burp and see the two important requests.
- One loads the available maps 
- the other is the GH set zip archive download. 

And keep in mind, our goal is to attack this app by simulating a machine in the middle attacker who can manipulate the clear text request or responses. Of course, we could simply intercept the traffic with Burp

or even use match and replace rules, but all of that fails once we try to handle binary data such

as the zip file and also we have to create the malicious zip file in the first place as well.

That's why we need a different solution. There are lots of ways how you could set up a testing environment now, but I'm going

to show you how I would do it. First, I'm going to Install a burp extension called HCDP Mock. This extension gives us a lot more freedom in the way how we can change and manipulate the responses to certain requests. To do that, we can right click our URLs

and select extension HGDP, mock, mock HGDP response and send these details over to the extension. Over there. We can now change the details.

Here we have the mock rules using regular expressions. Different parts of the requests are matched. Basically it works this way.

If a request is coming into Burp, it will check the host, the port, the method, and even the file path.

And if it matches, then respond with this data instead. But we are going one step further. Now we are actually going to use the redirect to URL feature

to send the request to another web server. This gives us extreme flexibility. So when we request the map JSON data,

we are actually sending a request to this Local server, URL, and if we match any of the map download request,

I'm also removing part of the URL matching in order to match any file that we download. Then we are going to request the zip file from this

local server, URL. Also, I'm changing some other fields here so they match anything. This way we can make sure our rules really get applied to all the incoming requests. Okay, now we specify that we wanna forward the request to this local server, URL,

and this local server doesn't exist yet, so let's create that next. I'm using Python for this with the flash framework

because I think it's pretty simple. Then we create two routes, one that responds to the map JSON request and one to the map zip download request.

Here we can retum some malicious map files that we could download if we want. And here we now have to return the malicious zip file.

But first, let's test this code. We run the server and it'll listen. Now on port 1, 2, 3, 4,

and back on Android, we are restarting the app and load again the list of maps. And if the setup is correct, we should be able

n

s

th

to confirm here that our custom request redirection worked. We are only seeing one app exactly as we have specified in our fake server.

But now the big question, what about the zip file? Well, this is now your part. Implement the code here to create a malicious zip file

that leads to an arbitrary file, right? Specifically try to write the hex file into the pocket maps download folder.

If you succeed to write this Hex file, the app will show you the flag to solve this lab.

### References
[Static vs. Dynamic Analysis](https://app.hextree.io/courses/network-interception/case-study-pockethexmap)

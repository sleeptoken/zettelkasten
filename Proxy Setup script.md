
2024-11-06 16:41

Source: #android 

documented in detail -> [[SSL Interception]]
current method shows a shortcut to setup a MITM for [[Cleartext Traffic Leads to Zip Path Traversal]]


So let's get started with Burp Suite as our proxy tool of choice. We are going to visit the local proxy port in order

to download the Burp certificate. And then let's head over to our Android emulator. I'm using here Android 14 without play services

so we can get access to root. And now we do the proxy setup. As we have seen before. We copy over the Burp certificate

and install it into the user certificate store. Then we use a DB with root access to run the HDP toolkit script

to inject our user certificates into the system certificate store of every app. This allows us to perform SSL interception,

even though in this case it's not really necessary because our target app is using clearex communication, but it's good to practice the general setup.

Next, we are going to set up our proxy using the VPN Trick with the Rethink DNS app. First, configure the DNS to use the system DNS

and then specify A-H-D-P-S Connect proxy pointing at the IP of our computer where Burp is running. Then that start the VPN and we should be ready to Intercept network request, which we can confirm using the web view test app. If this site doesn't load

and remains wide, your proxy set up falled somewhere. 

### References

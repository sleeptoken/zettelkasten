
2025-06-18 10:39

Source: #hardware 

Enable Bluetooth HCI snoop log from the Android Developer settings. HCI stands for host controller interface. it is essentially the communication between our host, which is android and our controller, which is the actual Bluetooth chip in the phone and vice versa. 
- This essentially creates a packet capture for us of all Bluetooth communication that was going on. 
### Loading the HCI snoop log in Wireshark

on modern android we couldn't just [[Android Debug Bridge]] pull these files.
use the following to generate a zip file, this will take a while.
```powershell 
adb bugreport
``` 
after extracting navigate to `FS-> data -> log -> bt` to find `btsnoop_hci.log`

- This isn't a text log instead it's a binary log containing full packet capture
### Lab

While reversing the hexlock app, we were understanding parts and renaming the variables and function.
there we stumble across 2 APIs.
1. API that takes username and password as json and returns an ID
2. API that return bluetooth ID and takes the above returned ID as a parameter

In order to hit those API's use [[curl]] 
```sh
curl -d '{"username":"stacksmash", "password":"test12"}' http://hexlock.hexoak.com:5001/user/locks -H 'Content-Type: application/json'
```
above command returns an array `[305,400]`
we use this to hit the `getbyid` API.
```sh
curl http://hexlock.hexoak.com:5001/padlocks/get-by-id/305
```
this returns a json object with `bluetooth_mac`, here we have the [[IDOR]] vulnerability, as we can iterate over the last `305` parameter and the API returns different `Bluetooth_mac` because of lack of authentication.

- see how the lock data is actually used in the app.

> Log messages are our biggest friend when reverse-engineering - They often contain original class names and information that helps us understand what a method or class is doing.

in the source code we can see lot of log messages have `BLELOCK` so we can assume that this class is `BLELOCK`, sometimes the compiler will inline other classes, in those cases we might see multiple different tags in the same class






### References
[hextree.io](https://app.hextree.io/courses/android-bluetooth-reversing/bluetooth-basics)
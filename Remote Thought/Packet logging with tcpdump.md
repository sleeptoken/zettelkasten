
2024-09-02 12:32

Source: #android 

Tags: [[Wireshark]]
### Setup emulator w capturing enabled

in order to see http request from an app we can use Wireshark (preferred method is using a proxy tool)

get the `AVDID` of the emulator from device manager of android studio and shut it down in android studio cause we need to start manually 

```bash
C:\Users\Keshav\AppData\Local\Android\Sdk\emulator\emulator.exe -tcpdump emulator.cap -avd Medium_Phone_API_35
```
- above code has a tcpdump flag, specifying a file name where to write the package too
- with the AVDID flag, we specify the ID for which android device to start, this launches a new android emulator window with network packet capturing enabled. 
after interacting with our app in the emulator, open the captured file in Wireshark.
### Wireshark file analysis

just after boot - data listed in the order as seen 
1. DNS query going out asking for google.com, and the connectivity check domain or even time.android.com
2. ADB related traffic, because that's how the emulator VM communicates with Android studio 
#### ClearText HTTP
Further filter http traffic, we see cleartext http request 
#### HTTPS (only domain name is visible)
- for secure https traffic filter TLS, we see a lot more packets but they are encrypted 
- we can still see some domain information, while we don't see the full URL, we still know the website they are interacting with 
### Eavesdropping attack (MITM)

This means that if we would see an attacked sending and receiving sensitive data in clear text, that would be a security issue, using tcpdump we can confirm whether this is happening or not

> keep in mind, just because there are clear text HTTP requests, it doesn't have to be a vulnerability.

- The connection check is a great example for that. 
- Android itself sends a clear text HTTP request, but just to check whether internet access works, for example, to check whether there is a WiFi captive portal or not. And you couldn't do that with HTTPS traffic due to various things that can go wrong there. 
- So this clear text traffic is not a vulnerability, but we could argue that our example app is vulnerable because an active attacker performing a machine in the middle attack who is sitting in the network, who can modify the clear text responses, they could change the response here and that is displayed to the user. 


### References
[Packet Logging with tcpdump (hextree.io)](https://app.hextree.io/courses/network-interception/android-networking-basics/packet-logging-with-tcpdump)
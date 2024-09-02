
2024-09-02 12:32

Source: #android 

Tags: [[Wireshark]]

in order to see http request from an app we can use Wireshark (preferred method is using a proxy tool)

get the `AVDID` of the emulator from device manager of android studio and shut it down in android studio cause we need to start manually 

```bash
C:\Users\Keshav\AppData\Local\Android\Sdk\emulator\emulator.exe -tcpdump emulator.cap -avd Medium_Phone_API_35
```
- above code has a tcpdump flag, specifying a file name where to write the package too
- with the AVDID flag, we specify the ID for which android device to start, this launches a new android emulator window with network packet capturing enabled. 
after interacting with our app in the emulator, open the captured file in Wireshark.
### Wireshark file analysis

DNS query going out asking o


### References
[Packet Logging with tcpdump (hextree.io)](https://app.hextree.io/courses/network-interception/android-networking-basics/packet-logging-with-tcpdump)
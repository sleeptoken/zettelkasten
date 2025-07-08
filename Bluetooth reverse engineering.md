
2025-06-18 10:39

Source: #hardware 

Enable Bluetooth HCI snoop log from the Android Developer settings. HCI stands for host controller interface. it is essentially the communication between our host, which is android and our controller, which is the actual Bluetooth chip in the phone and vice versa. 
- This essentially creates a packet capture for us of all Bluetooth communication that was going on. 
### Loading the HCI snoop log in Wireshark

on modern android we couldn't just adb pull these files.
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


### References
[hextree.io](https://app.hextree.io/courses/android-bluetooth-reversing/bluetooth-basics)
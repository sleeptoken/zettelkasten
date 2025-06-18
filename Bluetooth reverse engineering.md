
2025-06-18 10:39

Source: #hardware 

Tags: 

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



### References
[hextree.io](https://app.hextree.io/courses/android-bluetooth-reversing/bluetooth-basics)
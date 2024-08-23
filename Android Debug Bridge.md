
2024-08-23 13:46

Source: #android 

Tags:  

ADB consists of 3 parts
- **Client**
	- Both client and the server run on the computer 
	- we mostly interact with the client
- **Server** 
	- we don't really interact or manage the server manually 
- **Daemon**
	- runs on the devices and its called ADBD
	- The ADB server running on the computer communicates with the daemon using USB or TCP/IP

`adb shell` - gives a real linux shell into android
`adb -d shell `- always gives the shell of the USB connected device 
`adb -s emulator-5554 shell` - gives the shell of the emulator device running on port `5554` 




### References
[Research Device & Emulator Setup (hextree.io)](https://app.hextree.io/courses/research-device-setup/the-android-debug-bridge-adb)
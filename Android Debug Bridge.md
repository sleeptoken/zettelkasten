
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

### Transferring Files

#### Push
```
adb push <local_file_on_computer> <target_path_on_device>
```
To transfer files between our computer and the device
#### Pull 
```
adb pull <file_path_on_device> [<optional_target path_on_the_computer>]
```
This will store Downloads in your current directory.
#### Android Studio Device Explorer

Android Studio has an integrated Device Explorer, which allows us to explore the device storage with a nice UI.
### Managing apps using adb

```
adb install <path to .apk>
```
Using adb install we can manually install packages using the command line.

```
adb shell pm list packages
```
Lists all installed packages - including system packages.

```
adb shell pm list packages -3
```
List only third party packages.

```
adb shell pm clear <package_name>
```
Clear the application data without removing the actual application.

```
adb shell dumpsys package <package_name>
```
List information such as activities and permissions of a package.

```
adb shell am start <package_name>/<activity_name>
```
Starts the activity of the specified package.

```
adb uninstall <package_name>
```
Uninstalls the specified application.

You can find the full documentation for pm [here](https://developer.android.com/tools/adb#pm).
### References
[Research Device & Emulator Setup (hextree.io)](https://app.hextree.io/courses/research-device-setup/the-android-debug-bridge-adb)
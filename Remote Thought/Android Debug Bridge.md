
2024-08-23 13:46

Source: #android 

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
### adb logcat

```
adb logcat -v <log_format>
```
Change the log format - for example using `brief` to get a more condensed version of the log.

example on how to read a log message - 

`I/WifiService( 324): startScan uid=10107`

- `I` - indicated severity of the log line. There are 7 different severities
- `WifiService` - tag of the log message, can be chosen by the app programmer and in most cases indicates the service or the activity from which the log message originated
- `324` - process id of the logging process
- the rest is the log message 
### Log Filtering

In some cases there can be lots of log entries which makes it hard to focus on the things that matter. For example if you are only interested in the logs produced by the `MainActivity`, you can use a log filter for that:

```
adb logcat "MainActivity:V *:S"
```

Filter format:
- `MainActivity:V` ensures that logs from the tag `MainActivity` with a severity of Verbose and above are logged
- `*:S` Ensures that all other Tags are ignored (as nothing will log with log-level Silent or above)
### References
[Research Device & Emulator Setup (hextree.io)](https://app.hextree.io/courses/research-device-setup/the-android-debug-bridge-adb)
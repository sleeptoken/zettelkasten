
2024-09-12 11:42

Source: #android 
### How an APK is created

1. Most native Android applications are written in either Java or in Kotlin. 
2. The Java and Kotlin code then gets converted into a `*.class` file using the Javac or the Kotlinc compiler. This class file is just regular Java bytecode.
3. Then an additional compiler called `d8` converts the `*.class` file into a `classes.dex` file.
4. The `dex` file contains Dalvik bytecode, a special bytecode format designed for Android 
	1. on old Android versions. This bytecode was run by the Dalvik virtual machine 
	2. on newer Android versions, something called art, The Android runtime, ahead of time compiles the Dalvik bytecode into a native code for the specific processor, or for example, the phone we are running on. 
	3. This ahead of time compilation makes the Installation a tiny bit slower, but it also Improves the performance of the application itself drastically. 

1. The `classes.dex` 
2. The Android manifest XML,
3. The resources such as pictures and so on, 
4. Cryptographic signatures
gets packaged together into a zip file and this zip file is already an APK we can just change the extension to `.apk` then we basically have an android application.  

The best place to start when looking at a new Android application is to look at the Android manifest.
The first thing we want to check out are the exported activities. These are basically the activities that can be started by other apps or even can be started by us using ADB.

The activity that gets started when we launch the application can be identified by the action, which is main, and the category, which is launcher in the intent filter for the activity. 
And so here we can see that main activity is the activity that will get launched when we launch the application through the launcher. 

### References
[What is an APK? (hextree.io)](https://app.hextree.io/courses/reverse-android-apps/working-with-apks-and-apktool)
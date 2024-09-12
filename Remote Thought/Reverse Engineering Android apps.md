
2024-09-12 11:42

Source: #android 
### How an APK is created

- Most native Android applications are written in either Java or in Kotlin. 
- The Java and Kotlin code then gets converted into a `*.class` file using the Javac or the Kotlinc compiler. This class file is just regular Java bytecode.
- Then an additional compiler called `d8` converts the `*.class` file into a `classes.dex` file.
- The `dex` file contains Dalvik bytecode, a special bytecode format designed for Android 
- on old Android versions. This bytecode was run by the Dalvik virtual machine 
- on newer Android versions, something called art, The Android runtime, ahead of time compiles the Dalvik bytecode into a native code for the specific processor, or for example, the phone. We are running on this ahead of time compilation makes the Installation a tiny bit slower, but it also Improves the performance of the application itself drastically. 

The `classes.dex` file together with the Android manifest XML, and also the resources such as pictures and so on, also cryptographic signatures gets packaged together into a zip file. 

### References
[What is an APK? (hextree.io)](https://app.hextree.io/courses/reverse-android-apps/working-with-apks-and-apktool)

2024-09-12 11:42

Source: #android 
# How an APK is created

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
# Packing APKs

We can repack APKs using `apktool b` and the resulting `.apk` will be written to the `./dist` folder.

However the new APK will not be signed, we need to sign it before we can install it on our device.
# Creating a Keystore

**Creating a keystore**

The keystore (`research.keystore`) is a file that will contain our signing key. When hacking it's not important to keep this in a safe place, however if you were signing production apps you would want to make sure to keep that key both safe and secure.

To generate a `research.keystore` with a key named `research_key`, run:

```sh
keytool -genkey -v -keystore research.keystore -alias research_key -keyalg RSA -keysize 2048 -validity 10000
```

You can leave everything on default, and just confirm that the information is correct with `yes`.

# Signing an APK

To sign an APK we use `jarsigner`:

```sh
jarsigner -verbose -sigalg SHA1withRSA -digestalg SHA1 -keystore research.keystore app.apk research_key
# On newer Android versions SHA1 signatures still get rejected. In that case simply use the default algorithms
jarsigner -verbose -keystore research.keystore app.apk research_key
```

Make sure to replace `research.keystore` with the actual path to you keystore and `app.apk` with the path to your packed `apk`.

# Troubleshooting Install Errors


**INSTALL_PARSE_FAILED_NO_CERTIFICATES:** There is still something wrong with the signature. Maybe you tried to install an unsigned apk or the chosen algorithm (eg. SHA1) gets rejected.

**INSTALL_FAILED_INVALID_APK:** Failed to extract native libraries

This error occurs with some versions of [[apktool]] if the app contains native-libraries. To fix it, edit the AndroidManifest.xml so that `extractNativeLibs` is set to `true`. Afterwards you need to repackage and re-sign your APK.

**INSTALL_FAILED_UPDATE_INCOMPATIBLE:** Package `io.hextree.reversingexample` signatures do not match previously installed version; ignoring!

You will get this message if a version of the app signed with a different key is installed on the device. The simple solution is to delete the existing app. 

**Failed parse during installPackageLI:**

> Targeting R+ (version 30 and above) requires the `resources.arsc` of installed APKs to be stored uncompressed and aligned on a 4-byte boundary'

This happens on newer apps, try this alternative method using `zipalign` and `apksigner` coming with the specific version build tools:

```bash
$ apktool b
$ [...]/build-tools/34.0.0/zipalign -p -f -v 4 ./dist/<apktool_build>.apk aligned.apk
$ [...]/build-tools/34.0.0/apksigner sign --ks ./research.keystore ./aligned.apk
```

### References
[What is an APK? (hextree.io)](https://app.hextree.io/courses/reverse-android-apps/working-with-apks-and-apktool)
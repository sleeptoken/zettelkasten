
2024-09-13 12:58

Source: #android 

APKtool is an open source tool written in Java
Now apktool cannot just extract our APK. It'll also disassemble it and even better, we can reassemble APKs and so we can, for example, patch something in our disassembly or even in the manifest or so, and then repack the APK

disassembling our pulled APK.
```
apktool d <path to the APK we want to disassemble>
```

We also have an `apktool.yml` file. This file contains Information that apktool needs to reassemble the APK. 

- `Smali` is the Icelandic word for assembly, and it's basically an assembler for the dalvik bytecode you can get a lot of information out of a `smali` assembly file. 
- There's also `baksmali`, which is the disassembler for a dalvik Bytecode, and that's exactly what apktool ran on our dex files. It basically disassembled all the dex files into `.smali` files. 

The best place to start when looking at a new Android application is to look at the Android manifest.
The first thing we want to check out are the exported activities. These are basically the activities that can be started by other apps or even can be started by us using ADB.

The activity that gets started when we launch the application can be identified by the action, which is main, and the category, which is launcher in the

02:16

intent filter for the activity. And so here we can see that main activity is the activity that will get launched when we launch the application

02:25

through the launcher. But this is not the only exported activity here. There's also secret activity, which seems to be exported.

02:33

So let's try to manually start that activity just like we did in the research setup video. We will use a DB.

02:40

Shell start followed by the package name slash and then a dot and the activity name to launch our secret activity.
### References
[Extracting APKs with apktool (hextree.io)](https://app.hextree.io/courses/reverse-android-apps/working-with-apks-and-apktool/extracting-apks-with-apktool)
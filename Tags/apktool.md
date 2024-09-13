
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


### References
[Extracting APKs with apktool (hextree.io)](https://app.hextree.io/courses/reverse-android-apps/working-with-apks-and-apktool/extracting-apks-with-apktool)

2024-09-10 22:34

Source: 

Tags: 

We have used [apktool](https://apktool.org/) before to unpack and repack an android app. So let's use it in order to inject a permissive [network security config](https://developer.android.com/privacy-and-security/security-config).

```bash
# unpack the target .apk
PS D:\tmp> .\apktool.bat d translate.apk

# modify the AndroidManifest.com to add a networkSecurityConfig
# create a permissive xml/network_security_config.xml
cd translate

# repackage the .apk
apktool b

# ensure the .apk is zipaligned
[...]/build-tools/34.0.0/zipalign -p -f -v 4 ./dist/translate.apk translate2.apk

# create a keystore to sign the apk
keytool -genkey -v -keystore research.keystore -alias research_key -keyalg RSA -keysize 2048 -validity 10000

# sign the apk with apksigner
[...]/build-tools/34.0.0/apksigner sign --ks ./research.keystore ./translate2.apk
```


### References
[Patching Network Security Config with apktool (hextree.io)](https://app.hextree.io/courses/network-interception/advanced-interception-tricks/patching-network-security-config-with-ap)

2024-09-10 22:34

Source: #android 

We have used [apktool](https://apktool.org/) before to unpack and repack an android app. So let's use it in order to inject a permissive [network security config](https://developer.android.com/privacy-and-security/security-config).

```bash
# unpack the target .apk
PS D:\tmp> .\apktool.bat d translate.apk
```

- open the file in VS code
- modify the AndroidManifest.com to add a `networkSecurityConfig` add the following
```
android:networkSecurityConfig="@xml/network_security_config
```
- create a permissive xml/network_security_config.xml
- add a config that accepts user certificates find it in [[SSL Interception]]

next we have to bundle the file back into an APK again 
```shell
cd translate
apktool b
```
this APK is unsigned which means it cannot be installed, 

create a keystore to sign the apk
```
keytool -genkey -v -keystore research.keystore -alias research_key -keyalg RSA -keysize 2048 -validity 10000
```

ensure the .apk is zipaligned
```
[...]/build-tools/34.0.0/zipalign -p -f -v 4 ./dist/translate.apk translate2.apk
```
zipalign tool is a part of the installed built tool for the specific SDK 

sign the apk with apksigner (also part of the buildtools )
```
[...]/build-tools/34.0.0/apksigner sign --ks ./research.keystore ./translate2.apk
```
after doing all these steps we have a translate APK that trusts user certificates 
after installing the APK on ou 


### References
[Patching Network Security Config with apktool (hextree.io)](https://app.hextree.io/courses/network-interception/advanced-interception-tricks/patching-network-security-config-with-ap)
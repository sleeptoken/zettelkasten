
2024-09-10 22:34

Source: #android 
### Patching Network Security Config with [[apktool]]

We have used [apktool](https://apktool.org/) before to unpack and repack an android app. So let's use it in order to inject a permissive [network security config](https://developer.android.com/privacy-and-security/security-config).

```bash
# unpack the target .apk
PS D:\tmp> .\apktool.bat d translate.apk
```

1. open the file in VS code
2. modify the AndroidManifest.com to add a `networkSecurityConfig` add the following
```
android:networkSecurityConfig="@xml/network_security_config
```

3. create a permissive xml/network_security_config.xml
4. add a config that accepts user certificates, find it in [[SSL Interception]]

next we have to bundle the file back into an APK again 
```shell
cd translate
apktool b
```
this APK is unsigned which means it cannot be installed, 

create a keystore to sign the APK
```
keytool -genkey -v -keystore research.keystore -alias research_key -keyalg RSA -keysize 2048 -validity 10000
```

ensure the .apk is zipaligned, zipalign tool is a part of the installed built tool for the specific SDK 
```
[...]/build-tools/34.0.0/zipalign -p -f -v 4 ./dist/translate.apk translate2.apk
```


sign the APK with apksigner (also part of the build-tools )
```
[...]/build-tools/34.0.0/apksigner sign --ks ./research.keystore ./translate2.apk
```

after doing all these steps we have a translate APK that trusts user certificates 
after installing the APK on our play emulator we see the translate API request on burp  
but the request doesn't have an API key and we get an error 400
#### Patching with apktool - Lessons learnt

- the process of patching can break things or there are components relying on the correct signature of the app and now that it's self-signed, their permissions fail.

- This method can also be used to get around pinning and even patch other custom verification code if necessary. And this is really cool because it means we can intercept apps on devices without root access.
### Advanced HTTP Interception with VPN

In the Wi-Fi connection setting, we are able to set an HTTP proxy, but applications might ignore that setting. In some apps `NO_PROXY` is set (This HTTP request ignores system proxy settings). 
So even though we have installed our system certificate and configured the proxy in the WIFI settings, this request is not captured by Burp Suite. 

> **A VPN is intended to redirect traffic and we want to redirect traffic to our proxy.**

we require an application implementing an Android VPN service, and I'm using here the open source Rethink DNS app [GitHub - celzero/rethink app](https://github.com/celzero/rethink-app) (very flexible in the configuration)
#### Setting up the VPN

1. Open the app and go to Proxy > Setup HTTP(S) Connect Proxy 
	configure an HTTP(S) Connect proxy by specifying our burpsuite proxy URL.

VPNs can be applied to specific apps. This makes it a lot more versatile than Wi-Fi proxy settings 

2. go into the configuration (@ bottom) and change the DNS config to system DNS
3. Also make sure you have your proxy certificate installed in the system certs store.
Now we can start the VPN.

As you can see, the Android VPN features are really useful for interception, and by the way, that's also what the [[HTTP toolkit]] uses. 

### References
[Patching Network Security Config with apktool (hextree.io)](https://app.hextree.io/courses/network-interception/advanced-interception-tricks/patching-network-security-config-with-ap)
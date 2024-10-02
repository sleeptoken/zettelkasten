
2024-10-02 16:00

Source: #android 
### Setup

To install Frida & also want to install Objection, run:
```
pip3 install frida-tools
pip3 install objection
```

**Troubleshooting**
- add the user path variable in the environment variables if `frida --version` doesn't work 
- make sure [[apktool]] is installed
- if we get an error asking us to install `aapt` add` C:\Users\Keshav\AppData\Local\Android\Sdk\build-tools\35.0.0`(location of `aapt` and `appt2`) to user path 
### Patching APKs with Frida

our goal is to patch this Frida target APK to include Frida.
To inject Frida into an APK we can use objection:

```shell
objection patchapk -s apk_name.apk
```

Objection will extract, patch, re-pack, align and sign the application, and so it's a very fast and easy way to get Frida running.

Note that the application will wait on launch for Frida to connect to it, This is normal because the way Objection patches our application is that it waits for us to connect to the application using Frida before it actually fully launches the application. so to start the application we have to run:

```shell
frida -U FridaTarget
```

The `-U` here specifies that we want to connect by USB. 
`FridaTarget` is the name of the app or the PID of our application
###  Running the Frida Server

- An alternative to patching the APK is to run Frida server on our device. ( Frida server requires a root device )
- makes interfacing with applications very easy 'cause you don't have to patch every single APK you want to work on 

If you have a rooted device, you can also run `frida-server` instead of patching the APK. You can download Frida-server on the [Github Releases Page of Frida](https://github.com/frida/frida/releases) ([frida-server-16.5.2-android-arm64.xz](https://github.com/frida/frida/releases/download/16.5.2/frida-server-16.5.2-android-arm64.xz)). Note that it comes `xz` compressed, so you have to extract it (`xz -d` on unixoid systems, 7zip on for example Windows).

To install it in an emulator we can `adb push` the server over:

```shell
adb push frida-server /data/local/tmp/
```

The reason we use this specific directory is that other path such as `/sdcard` are mounted no-exec and so we can't run applications from there.

Afterwards we want to run adb as root, and also make the server executable:

```shell
adb root
adb shell
cd /data/local/tmp
chmod +x frida-server
```

And then we are ready to go: We can launch the server by running

```
./frida-server
```

Now we can connect to the application by running:

```
frida -U FridaTarget
```


### References

https://app.hextree.io/courses/android-dynamic-instrumentation
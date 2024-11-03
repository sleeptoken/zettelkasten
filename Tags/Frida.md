## Verify the architecture 

Verify the architecture of your device to ensure you’re using the correct Frida server version. Run:
```
adb shell getprop ro.product.cpu.abi
```

This command will return the device’s CPU architecture, which may show options like:
    - `arm64-v8a` for 64-bit ARM
    - `armeabi-v7a` for 32-bit ARM
    - `x86` or `x86_64` for Intel-based devices or emulators

##  list all processes 

```shell
frida-ps -Uai
```
Run `frida-ps -Uai` to list all processes along with architecture information.

### Change Frida version 

the agent, the server, the client frida version and the frida-tools versions all need to be compatible with each other 

```powershell
pip3 install "frida-tools==9.2.5"
```

download the correct frida server version if you are using server method
is using objection u can specify the frida version that objection uses when patching a apk.
```
objection patchapk -V 14.2.8 -s 2048.apk
```
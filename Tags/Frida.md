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
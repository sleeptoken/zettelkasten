
2024-10-02 16:00

Source: #android 
### Setup

To install frida & also want to install Objection, run:
```
pip3 install frida-tools
pip3 install objection
```

**Troubleshooting**
- add the user path variable in the environment variables if `frida --version` doesn't work 
- make sure [[apktool]] is installed
- if we get an error asking us to install `aapt` add` C:\Users\Keshav\AppData\Local\Android\Sdk\build-tools\35.0.0`(location of `aapt` and `appt2`) to user path 
### Patching APKs with Frida







### References

https://app.hextree.io/courses/android-dynamic-instrumentation
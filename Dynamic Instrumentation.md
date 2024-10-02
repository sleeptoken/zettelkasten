
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

Now, as mentioned before, we need to somehow inject the Freeda agent into our application. And the easiest way to do so is using objection.

In this course, we will mainly work with a special application I built called freeda Target. And so our goal is to patch this freeda target a PK

to include Freeda. This is super simple to do with objection.

We can simply say objection patch A PK dash S,

and then our A PK name. So freeda, target apk, hit return, wait a bit, and then this generates a new A PK for us.

The FREEDA objection dot AP K. This APK will already be fully signed and so on and so forth and is ready to be installed.

And so let's do just that. So I'm gonna type a DB Install freeda Target objection, apk. And now if we check now emulator, we can see

that indeed our Freeda target application is there. Now as we open up that application, you can see that simply a blank screener.

This is normal because the way Objection patches our application is that it waits for us to connect to the application using Frida

before it actually fully launches the application. So let's try to connect using Frida. We can simply do Freeda dash uppercase U for USB.





### References

https://app.hextree.io/courses/android-dynamic-instrumentation
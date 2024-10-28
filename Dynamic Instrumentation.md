
2024-10-02 16:00

Source: #android 
### Setup

To install Frida & also want to install Objection, run:
```
pip3 install frida-tools
pip3 install objection
```
#### Troubleshooting

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
### Frida REPL

The Frida REPL (Read-Eval-Print-Loop) is a JavaScript interpreter, and so we can directly run JavaScript statements:

```js
for(var i=0; i < 5; i++) { console.log(i); }
```

To create multi-line statements, suffix each line with a `\` backslash:

```js
for(var i=0; i < 5; i++) {\
    console.log(i);\
}
```

You can also search through your command history by pressing` Ctrl+ R`, 
for example type `for`, and then it will find all lines that you've executed that contain `for`.

If you want to find an earlier command, just hit `Ctrl+R`. Again, 

There are also a couple of built-in non JavaScript commands, and those can be accessed by typing `%`.
You can, for example, say `%help` to get a list of all available commands. 
### Frida Script

To load scripts with Frida, we can just start Frida with the `-l` option:

```js
frida -U -l test.js FridaTarget
```

We can enable and disable auto-reload by doing:

```js
%autoreload on/off
```

To manually reload all scripts, we can just run `%reload` in the REPL.

if we edit our script and for example, add a second log message and hit save, then you can see that our script is automatically reloaded. 
**Now depending on your combination of editor and operating system, you will see that sometimes Frida will auto reload the script multiple times.**
we can disable the auto reloading by adding `--no-auto-reload` to the arguments of Frida. 

we can also just say `%reload` to reload our script at runtime.  

functions that we define in our script will be available in our Frida REPL. So for example, let's say we create a function test. if we reload the script here manually (`%reload`) and now we call `test()` you can see that the function we just defined can be executed. 
### Instantiating Objects and calling methods 

#### Java.use

We can get JavaScript wrappers for Java classes by using `Java.use` This wrapper even allows us to instantiate instances of that class. 

```js
var sc =  Java.use("java.lang.String")
```
This will give the class `Java.lang.String` or the regular string class of Java as a JavaScript wrapper

```js
sc.$new("I am a Java String!")
```
`$new` - which is the Frida way of calling the constructor.
`I'm a Java string`-  as my first argument to the constructor.
this will create an instant of `Java.lang.String`.

we will assign the instance to a variable `si`.
```js
var si = sc.$new("I am a Java String!")
si.toString() //take a look at the contents of the string Instance
si.charAt(0) // let's call one of the instance methods
si.$dispose() //We can dispose of instances (for example to free up memory) using `$dispose()`, however this is almost never required, as the Garbage Collector should collect unused instances.
```

If we want to know which classes are actually available, we can call `Java.enumerateLoadedClasses()`. Note that there are two different versions of this function.

One will call a callback for each class that is loaded - `enumerateLoadedClasses()`
one will return an array of all classes that are loaded. - `enumerateLoadedClassesSync()`
#### Replace the implementation of a certain function

What's also really cool is that we can actually replace the implementation of a certain function. 
for example, if we want to replace the charAt implementation

```js
sc.charAt.implementation = (c) => {\
... console.log("Custom charAt!");\
... return "X";\
... }
```

try to again call `charAt()`, on our previously created string instance and We get our custom log message and just an "`X`" is returned. 
### Mixing Static and Dynamic Analysis & Java.perform

when we decompile the `FridaTraget` APK we see that we only have one exposed activity 
navigating to `source code> io.hextree > fridatarget > ExampleClass` we see a function defined under a` Public class ExampleClass`

we can right click the class and select `Copy as frida snippet`

if we just paste the frida snippet in the frida repl we get an error saying that it couldn't find the class. The reason for this error is the way that our frida to Java Bridge works under the hood.

Essentially, the code executed in the normal frida repl does not run directly within the main threat of our application.

Therefore we have to use something called `Java.perform` to ensure that Frida executes the code in VMs thread where all classes and so on, have already been loaded and are easily accessible. 

`Java.perform` takes in a function as its first argument and in that function we can actually just paste in our code and we'll we'll be able to run it without any error.
#### To connect frida to a script 

open cmd in the directory of the script and enter
```js
frida -U -l script.js FridaTarget
```

contents of `script.js` for a function that takes a parameter 
```js
Java.perform(() =>{
	let ExampleClass = Java.use("io.hextree.fridatarget.ExampleClass");
	let ExampleInstance = ExampleClass.$new();  //instancee of example class
	console.log(ExampleInstance.returnDecryptedString()); //for function w/o parqameter
	console.log(ExampleInstance.returnDecryptedStringIfPasswordCorrect("verysecret")); //for function w parqameter
}) 
```
### Tracing Activities

 Let's write ourselves a useful frida script that will always tell us which activity is currently in the foreground.

To do that, let's start by looking at the ([documentation](https://developer.android.com/reference/android/app/Activity#activity-lifecycle)) for activity. If we scroll down on this page, we can find the lifecycle documentation for activities. `onResume()` is called when the app is for example, brought forward from the background or when activity is switched. And so it would be the perfect function to customize to look out which activity is currently active.

a script to trace the active Activity:

```javascript
Java.perform(() => {
    let ActivityClass = Java.use("android.app.Activity");
    //replace the implementation of the on resume function.
    ActivityClass.onResume.implementation = function() {
        console.log("Activity resumed:", this.getClass().getName()); // log activity name to us
        // Call original onResume method
        this.onResume();
    }
})
```
### Tracing Fragments

However, if we switch to the different screens of the application, we don't get any new activity logs. This is because these different screens are based on `fragments`. 
But if we check the ([Documentation](https://developer.android.com/reference/androidx/fragment/app/Fragment)) for fragment, we can see that they have similar lifecycle functionalities. And so which should be easy to write a similar script, but for `fragments`. And to do that, we first need to find the full class name for the fragment 

`fragments` have been deprecated on API level 28, but there is a new replacement that has the same API
on clicking [view source](https://cs.android.com/androidx/platform/frameworks/support/+/androidx-main:fragment/fragment/src/main/java/androidx/fragment/app/Fragment.java?q=file:androidx%2Ffragment%2Fapp%2FFragment.java%20class:androidx.fragment.app.Fragment)we can see the full path as `androidx.fragment.app`

The finished script to trace the active fragments:

```javascript
Java.perform(() => {
    let FragmentClass = Java.use("androidx.fragment.app.Fragment");
    FragmentClass.onResume.implementation = function() {
        console.log("Fragment resumed:", this.getClass().getName());
        // Call original onResume method
        this.onResume();
    }
})
```
### Tracing with Frida

#### Frida-trace

With Frida Trace you can trace function calls in the application 

We use the fully qualified class name, then exclamation mark, and then the method name.
```js
classname!methodname 

io.hextree.Class!button_click_handler //to trace the button click handler
io.hextree.*!* //match all methods on all classes under io.hextree.
*!*passsword* //match on all functions that include password
```

##### To trace all calls on `io.hextree.*` 
```js
frida-trace -U -j 'io.hextree.*!*' FridaTarget
```

 if we switch to the tracing menu item and then click a button, weirdly enough, nothing happens. Even though this fragment is definitely in io.hextree. The reason for this is that the fragment wasn't loaded when we started Frida Trace. And so if we terminate Frida Trace now and start it again w the same command we can see the functions that were called

> The part before ! is the filter for the class-name, while the part after the ! is the filter for the methods
##### exclude classes

Now let's hit the second button and unfortunately, an annoying class spams our Frida Trace output. Luckily, Frida Trace also lets us exclude classes and methods by just specifying them with the `-J` argument.
```js
frida-trace -U -j 'io.hextree.*!*' -J '*AnnonyingClass*!*' FridaTarget //all entries of AnnoyingClass are gone
```
### Tracing into JNI

We can see the native calls, however, we don't by default see the calls that actually happen in the native library. 

Let's try to change that
in Jadx we will try to trace a native library named `libhextree.so` located at `Resources > lib > arm64-v8a`  

```js
frida-trace -U -I 'libhextree.so' -j 'io.hextree.*!*' FridaTarget
```

And now the instrumentation takes quite a while because it has to instrument all the functions that are in the native library, which includes the entire standard library. 

Now let's see what happens when we click the JNI button. Now there is a lot more output here. The weird function names following that are mangled c++ names.

this sometimes is really useful, but in a lot of cases it's also really, really noisy.
### Replace function arguments / return values returned by functions

- This can be used to bypass security controls such as license checks 
- even change different modes of the application, for example, into a developer mode Instead of a production mode. 
- We can also often use it to bypass functionality such as SSL pinning.

Interceptor does not really work with Java code, and so we basically have to build our own interceptor 

We can use Frida to intercept function calls and return values. For example, to replace the return value of `InterceptionFragment.function_to_intercept`, we can just write a simple script:

```javascript
Java.perform(() => {
    var InterceptionFragment = Java.use("io.hextree.fridatarget.ui.InterceptionFragment");
    InterceptionFragment.function_to_intercept.implementation = function(argument) {
        this.function_to_intercept(argument);
        return "SOMETHING DIFFERENT";
    }
})
```

#### Alternate
``` js
InterceptionFragment.function_to_intercept.implementation = function(argument){
        console.log("fuctioncalled: ",argument);
        argument = "something else";
        return this.function_to_intercept(argument);
    }
```
### SSL Validation Bypasses

#### SSLContext & Network-Security-Config Bypass

If you have looked at our Android network analysis course, you encountered a lot of SSL validation and different ways to bypass or modify that validation.

Another way to disable SSL validation and also to bypass certificate pinning is to use free data to just disable it.

Now the way we bypass SSL opening on Android depends on how it's implemented. One common way to implement a opening

and validation is to explicitly set up a key store with a single trusted certificate. This key store is then used to create a trust manager

and that one will only validate this single certificate. If we check the documentation for X 50 9 trust manager,

we can find this check server trusted method right here. And this method seems to validate the certificate chain and we'll throw an exception if the certificate is not

trusted by the trust manager. So it sounds like the perfect function to replace to disable SSL opening and even SSL validation altogether in our free data target,

we have an SSL opening view, and in here we have three buttons that will all create a network request

and they all use a different method for pinning the certificate. Our goal will be to bypass all three of these.
essentially they currently have the wrong certificate pinned and so the request falls if we manage to bypass the SSL opening

or SSL validation altogether, the buttons should turn green. Now let's start by using Frida Trace to find Invocations to the function check server.

Trusted that we saw earlier. If we click the first request, now we can see that platform check server trusted is called

and that it has quite a long signature. This will be the implementation that we will try to replace, but unfortunately the package name is missing here.

So the stuff that comes before the platform, luckily Frida comes with a nice way to enumerate all classes and methods and so on.

And so let's drop into Frida Apple and then we just do Java, do enumerate methods, and then we can use the same features

and text that we also can use for Free Trace. And so I will just filter for Star Platform exclamation mark,

star check server, trusted Star. And in the results I can see that the package name is com.androld.org dot conscript do platform.
'm gonna copy that one and then we will write a small script. We start by just our typical Java platform

and then we will get the JavaScript Rev hub for the platform class. Next, we simply try to replace the check server trusted

function with our own implementation. And then we are already ready to test. So let's use three.run our script and we get an error message.

There are two different overloads for Check Server Trusted, and so we have to tell Frida which one we want to use.

Now, nicely enough, Frida shows us bot available overloads and we can just copy paste this code in. I will just try the first one and hope that it works.

So let's add the overload call in here and I will hit save and just restart the script. And now there's no error message.

And so let's try the SSL context pinning function and it tums green. Nice. We just bypass the SSL validation just

by replacing the checks server, trusted implementation and making sure that it doesn't throw the certificate exception. Another way to set up SSL putting

and validation is to use the network security config. And down here you can see how, for example, a certificate might be pinned.

Now I thought that this might need a different bypass, but if wa imet tru it with our avleting errint




### References

https://app.hextree.io/courses/android-dynamic-instrumentation
Check-out the full Frida JavaScript API documentation [here](https://frida.re/docs/examples/javascript/)! Note that the number in parenthesis after each command Indicates the number of expected arguments. 
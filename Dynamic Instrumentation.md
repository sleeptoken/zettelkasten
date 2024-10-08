
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

that it automatically created. Let example class equals Java, use IO HX three Freeda target example class, super useful. However, if we hit return we get a large error saying

that it couldn't find the class. The reason for this error is the way that our freeda to Java Bridge works under the hood.

Essentially, the code executed in the normal freeda report does not run directly within the main threat of our application.

Therefore we have to use something called Java platform to ensure that Frida executes the code in DVMs threat where all classes and so on have already been loaded

and are easily accessible. Java platform takes in a function as its first argument and in that function we can actually just paste in our.code

and we'll we'll be able to run it without any error. Now doing this all the time, the Ripple is really not very ergonomic

and so this is one of those cases where a script really comes in useful. And so let's start Frida and connect it to a script.

We can quickly write and in that script we'll just say Java perform, create our function, paste In the example class code generated by Jet X

and now let's create an Instance of our example class. So I would just say example, instance equals example class

And then we should have our instance created and now we should be able to console log out the result

of the function return decrypted string. Let's savę this and we immediately get the log message. I'm very securely encrypted

and as we have odd reload enabled, you can see that our script gets reloaded a couple of time since we see it four times.

I think this was much faster than manually reversing whatever the flag crypto module does. Now the other function return decrypted string if password

correct, takes in a password string and checks that it's equal to very secret. And so let's try to also call that one.

I'll copy the full function name and then I will just create a second console lock. This time calling the retum decrypted string if password,

correct with the very secret argument. And now if we hit safe we can see that an other string appears. Luckily

I was password protected. So as you can see, Frida and dynamic instrumentation can really save you a lot of time when reversing something.

You can just call some functions at runtime and see what they do. And now it's your turn. In the application you will also
### References

https://app.hextree.io/courses/android-dynamic-instrumentation
Check-out the full Frida JavaScript API documentation [here](https://frida.re/docs/examples/javascript/)! Note that the number in parenthesis after each command Indicates the number of expected arguments. 
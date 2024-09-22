
2024-09-16 20:44

Source: #android 
### WHY 

Because of features such as `reflection`, Java applications generally retain all their symbols. Also, because the Dalvik bytecode is relatively high-level, it contains a lot of information on the structure and control-flow of classes. This means that we can get very good results when decompiling Android applications.
### JADX usage 

- Double click the name of the activity and it will automatically bring you to that class
- **Add a comment** -  semicolon or right click then `comment`

we have an if statement where an input string is compared to `R.string.secret2` on clicking secret2 it brings us to a class where the variable is defined with a number assigned to it. These strings being retrieved from the compiled in resources and so we need to somehow find those. 
- **search function** - just right click secret2 and say Global search. By default, the global search will only search through the code, and so we also want to enable resource here. and we get the flag
- we can switch to `Smali` code from bottom left 
### JNI - Java Native Interface

Used to call into native code, commonly used to embedded C or C++ code or libraries into an application.

This time the password is compared to `secretFromJNI()`
```java
package io.hextree.example_nativelib;
public class NativeLib {
	public native String secretFromJNI();
	static {
		System.loadLibrary("example_nativelib");
	} 
}
```

JNI allows us to call into native platform and CPU dependent code, and so for example, Into `C` or `C++` code.

There are a lot of different reasons for why somebody would use JNI.
1. One is performance. 
2. The other one is to write cross-platform code that also runs, for example, on iOS 
3. sometimes also just the toolkit that was used or the library that was used is only available as `C` or `C++` library. 
 
We can find the native libraries under resources lib, and you can see that for each platform `arm64`, `armeabi`,` x86`,` x86_64` there is a shared object here.

We can't analyze this shared objects in JADX directly but we can, for example, load it into [[Ghidra]] or just [[strings]] on shared object
### Saving in JADX

When Saving a JADX project, it's stored as a `.jadx`file, and this file will contain all the decompiled sources of the APK. It's a good file format if you want to send your reverse project to somebody else.

If you have done **modifications** to **symbols** in jadx, you can also export your mappings as `Tiny v2` or as `Enigma` file.
You can also save all the decompiled sources to a folder. You can then, for example, load it into other tools.

You can export the entire project as a `Gradle` project. 
Gradle projects are especially useful when you want to import the project into a different IDE and read the code there. So, for example, Android Studio natively supports Gradle projects.
Often when you import a project, there will be errors and warnings. In a lot of cases, you can Just Ignore those but sometimes something goes wrong and you wanna check the error log to see whether you can spot what's going on.

If you prefer to use a different IDE, you can also export the entire decompiled source, even as a Gradle project (but it's buggy and often doesn't work).

Jadx also has a powerful CLI that you can use to decompile APKs headless. You can find the CLI option documentation [here](https://github.com/skylot/jadx?tab=readme-ov-file#usage).
### Important steps for Reverse engineering 

- **Define clear research goals** - for our weather app we want to find the API endpoint from where the app is fetching data - we will directly search for "`http`" in global search
- Break research goals into smaller goals, much like in life. 

> **Focus on your goal, not the code.** If you know you are looking for a specific functionality it's often best to search for that functionality directly instead of trying to understand the overall code & structure of the application.
### Case study of the Weather app
 
After a bit of reverse engineering we can see that the API key is hidden in a string resource `res>values>strings.xml`- something you will commonly see in applications in the wild.
#### Identifying the Correct ZIP Code

Like always in reverse engineering there are many paths that can lead to the goal 
##### Method 1 (Trace "Done" button click)

start from UI and try to trace where the entered zip code goes 
further down the line we find a method (`m.e()`) getting called, this method gets a reference to the shared preferences.
###### Shared preferences.
These are application specific preferences that the app wants to persist and store even when the app is killed. So this is kind of like a database intended for user preferences.

So we get the shared preferences and then we store the zip code in it. Looks like this whole class is related to preferences. So we can rename the class now

> Renaming classes and variables during reverse engineering always helps

if the zip code is stored in the preferences. Where's the zip code? Read and used?
Select the method that loads the zip code from the preferences and with `Find Usage(x)` we can see where it gets used. 
We can find three places in main activity. And when we go through them one by one, we eventually see an if case, checking whether the zip code is 1, 3, 3, 3, 7, or 42.

##### Method 2 (Find Error Message) 

we look up the text of the error message. search for Whether updates using search feature
#### Crafting the API request 

in the code we see this 
```java
new e(str, this.f402a).start();
```
First, `this` value turns out to be the `MainActivity` object.
##### this.f402a

It's actually somewhat common to see activity objects passed around in Android apps.
- The reason for that is that several Android APIs or functions require a context and the activity is such a context.

For example, the `toast` message used here for the error message, it requires a context and it got passed in `this`.
```java
if (!b2.equals("13337") && !b2.equals("42")) {
    Toast.makeText(this, "Weather Updates Disabled", 0).show();
    return;
}
```

And `this` is the main activity. 
##### .start()

Keep in mind the app is obfuscated with `proguard`. So normal method names should be obfuscated and shortened. So the fact that it says in clear `start` this can tell us that this might be a standard library call, which we can actually confirm if we follow the chain.
### Diffing Application Updates

Updates usually include new features which could introduce new security issues or an update fixes an issue of a previous version

and to figure out what has changed can be done with **diffing**
the Jadx DECOMPILER also has a command line tool which can be used to decompile into a folder.
```shell 
./bin/jadx io.hextree.weatherusa.apk -d app1
./bin/jadx io.hextree.weatherusa_update1.apk -d app2
```

 enable an extension for VS. Code `Compare Folders`.
 - It works by opening one of the folders in VS code 
 - then go to the extension and select the second folder. 
 
this method of Diffing app updates can be quite useful to find interesting areas to investigate, especially in apps you have never looked into. It can guide you to interesting new areas
#### Function with native keyword  

Just directly search for the X-API key in the updated app, which brings us here and this shows us that the API key seems to be returned by this function `getKey` and a weird string. And where is that function? Well, It's not here. The function appears empty. The reason for that is the native keyword.

This function is Implemented in native code in a binary, which we can also see here at the top of the class here.

Native libraries In an app can be found in the lib folder and in this case it contains binaries for four different architectures. all these libraries contain the same code, just Compiled for different architectures

 I can generally identify three different strategies how to do that.
- First you could use a disassembler tool like Ghidra tool, 
- Another strategy is network Interception. 
- Just run the library code
#### Just run the library code


### References
[Reverse Engineering Android Apps (hextree.io)](https://app.hextree.io/courses/reverse-android-apps/decompiling-android-applications)

To install jadx, follow the instructions on the [jadx Github Repository](https://github.com/skylot/jadx?tab=readme-ov-file#download).
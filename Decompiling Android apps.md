
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
##### Method1

start from UI and try to trace where the entered zip code goes 
further down the line we find a method (`m.e()`) getting called, this method gets a reference to the shared preferences.
\
###### Shared preferences.
These are application specific preferences that the app wants to persist and store even when the app is killed. So this is kind of like a database intended for user preferences.

So we get the shared preferences and then we store the zip code in it. Looks like this whole class is related to preferences. So we can rename the class now and of course we can start renaming all the other things to make it clearer. For example, here we get this string out of the preferences. So this string is the zip code and this function returns it, meaning we can rename this whole method to get zip. So just by reading the functions,

02:25

we can deduct their intention and we can come up with fitting names. Of course, these are not the real names of the source code.

02:32

These are just names we give them. Sometimes our assumption can be wrong, but here I have a good feeling these names make sense.
And this is also the end of the button click. But this gives us new goals to follow if the zip code

02:58

Is stored in the preferences. Where's the zip code? Red and used? Well, here's the method that loads the zip code from the preferences

and with X we can see where it gets used. We can find three places in main activity. And when we go through them one by one,

03:06

we eventually end up here. Here we get the zip code string followed by an if case, checking whether the zip code is 1, 3, 3, 3, 7, or 42.

03:14

By the way, if we had chosen a different research strategy, instead of starting with the zip input field, we looked up the text of the error message.

03:26

Whether updates this enabled, we could have immediately found this code, but we obviously didn't know this at the start.

03:33

Both approaches are perfectly valid. In one case, we would have found the answer about the zip code Immediately. With the other we didn't.

03:40

But along the way, we reverse engineered a few classes and functions and learned about the shared preferences used by this app.

03:47

So both paths have their advantages and disadvantages. Now the moment of truth, let's go

### References
[Reverse Engineering Android Apps (hextree.io)](https://app.hextree.io/courses/reverse-android-apps/decompiling-android-applications)

To install jadx, follow the instructions on the [jadx Github Repository](https://github.com/skylot/jadx?tab=readme-ov-file#download).
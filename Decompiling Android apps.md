
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
3. sometimes also just the toolkit that was used or the library that was used is only available as `C` or `C++` library. We can find the native libraries under resources slip,

and you can see that for each platfo Army AB X 85 X 80, 60, 64, ha detind shared object here.

We can't analyze this shared ob

but we can, for example, load it in

just run strings on the shared objec

to see whether we can find the string

after a lot of strolling, indeed, here's our

password. Let's go give a native attempt

secret,
### References
[Reverse Engineering Android Apps (hextree.io)](https://app.hextree.io/courses/reverse-android-apps/decompiling-android-applications)

To install jadx, follow the instructions on the [jadx Github Repository](https://github.com/skylot/jadx?tab=readme-ov-file#download).
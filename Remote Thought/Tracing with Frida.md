
2024-11-03 12:48

Source: #android 
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
- even change different modes of the application, for example, into a developer mode Insttead of a production mode. 
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
### References
[[Dynamic Instrumentation]]
 
2024-11-29 21:13

Source: #android 

The development documentation for [Activity](https://developer.android.com/reference/android/app/Activity) explains it like this:

> _"An activity is a single, focused thing that the user can do. Almost all activities interact with the user, so the Activity class takes care of creating a window for you in which you can place your UI"_

When you open an app on your phone, any screen you can see has an Activity behind it, and it turns out that other apps can sometimes interact with these activities as well. Generally one activity implements one screen in an app.  
### Attack Surface

In order to attack an app, we need to understand what can we even interact with. That's why we should look at the `AndroidManifest.xml` and look for any `<activity>` with the `android:exported="true"` attribute.

`android:exported="true"` tells us weather other apps can interact with this app directly or not 

`android:exported="true"` is interesting to us ,`android:exported="false"` can be ignored
### Intent

Here is how the official documentation describes [Intent](https://developer.android.com/reference/android/content/Intent):

> _"An intent is an abstract description of an operation to be performed."_

But a more intuitive description could be: _"Declaring an intention to do something, and let Android figure out the app that can handle it"_

### Starting Activities

#### Basic exported activity

Activities are responsible to render the screen of an app. So if your app has multiple screens, you can use `startActivity()` to start another activity. To do that you have to create an Intent object and target a specific activity class.

the `SecondActivity` cannot be started from another app. Only the app itself can start this "Internal" activity
we can start a non exported (exported="false") activity from WITHIN the same app 
- how we generally prepare intents to call other apps

```java
Intent intent = new Intent();
intent.setComponent(new ComponentName("io.hextree.activitiestest", "io.hextree.activitiestest.SecondActivity"));
startActivity(intent);
```

An alternative syntax to specify the target package and activity is also: **Typical Internal Intent** 
typically code you see inside of apps starting their own internal activities

```java
Intent intent = new Intent();
intent.setClassName("io.hextree.activitiestest", "io.hextree.activitiestest.SecondActivity");
startActivity(intent);
```

An app can always start any of its own activities, but in order to allow other apps to start your activity, they have to be exported. There is also one exported activity by default, which is the "launcher activity" that is used as a main entry point into the app. This allows the home screen or launcher application to start your app when you click it.
##### Using adb shell

```powershell
adb shell am start-activity -n io.hextree.attacksurface/io.hextree.attacksurface.activities.Flag1Activity
```
#### Intent with extras 

actions are mentioned in the android manifest then use -a tag in the adb shell command 

### Incoming Intent

The intent object that was used to start an activity, is available to the app via `getIntent()`.This feature is used to pass data to other apps, and thus it becomes a major attack surface.
#### Intent Attack surface

- `Activity.getIntent()`
- `onActivityResult(int requestCode, int resultCode, Intent data)`
- `onNewIntent(Intent data)`

we can add things to an intent by using `intent.putExtra("hextree",1337);`
### Journey of an Intent 

#### Parcels

[[Parcels]] are a way to serialize and de-serialize objects. We are serializing the intent object, which contains target activity string and lots of different values

Android documentation definition - 

A parcel is a container for a message that can be sent through IBinder. A parcel can contain flattened data that will be unflattened on the other side of the IPC. So it's similar in concept to an intent. Intents are also just like a message, but they are higher level Java objects. parcels are really low level involved in the actual inter process communication between the different apps. 
#### Binder

Binder is a kernel driver. It sits deep in the Android operating system. 
If you access your phone or emulator with ADB and search for anything binder related using
```sh
find / 2>/dev/null | grep binder
```

There are binder related device files. These are fancy Linux files that actually expose the driver and you have native libraries that implement the interaction with these.

Now, if you run the apps on a phone or emulator with root access, you can also check the processes of your apps. 
```sh
su root
ps -A | grep hextree
```

So first of all, you see here the Android apps are running as their own Linux process. 

```sh
cat /proc/2417(process-id)/maps
```
With the process id, we can find the memory map of this app process. This app lots of stuff into memory and if you look through it, you will find `libbinder`,
> the native code library implementing the interaction with the binder driver, which allows the app to send data to other app process such as the intent when we call start activity.

Luckily we don't really have to interact with any of the low level stuff directly. That's the beauty of Android and the development SDK.
### Implementing Intent Debug Features



### References
[Intent Attack Surface](https://app.hextree.io/courses/intent-threat-surface/intents-and-activities)
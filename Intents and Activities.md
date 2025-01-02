 
2024-11-29 21:13

Source: #android 

Tags: 

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

We should proceed slowly by stepping into and out of functions following the journey of the intent that we want to send in here.

You can also find some interesting things. For example, there is a function prepared to leave process, which checks a few values of the intent we attempt to send.

What we see here are security checks to harden Android apps against an intent related vulnerability that could lead to exposing internal files.

But this is a more advanced topic we will cover later. For now, let's keep following the call and eventually we reach the get service start activity.

Call services are another important Android feature we will explore in another course. Then things like an interface class with this dollar step will make more sense.

So services are also involved here. If we keep stepping, we eventually land in some code related to parcels and writing parcel data.

This means we are slowly reaching the low levels of Android parcels are a way to serialize and des serialize objects

and we are serializing here. The intent object, which contains lots of different values, including also the target activity string.

A parcel is a container for a message can be sent through binder. A parcel can contain flattened data that will be unflattened on the other side of the IPC.

So it's similar in concept to an intent. Intents are also just like a message, but they are higher level.

Java objects parcels are really low level involved in the actual interprocess communication between the different apps. And you can also see that in some of the methods

that start to show up. For example, this native right strong bin function, this is a native method, so we are heading into c plus plus Android internal code.

But long story short, eventually this all leads to binder. You maybe have seen this word showing up here and there.

And binder is a kernel driver. It sits deep in the Android operating system. If you access your phone or emulator with a DB

and search for anything binder related, you can see it here. There are binder related device files. These are fancy Linux files that actually expose the driver

and you have native libraries that implement the interaction with these. Now, if you run the apps on a phone

or emulator with root access, you can also check the processes of your apps. So first of all, you see here the Android apps are running

as their own Linux process. This is the user id, the process runs it, so you can see they run as different

Separate Linux users. And here's the process ID which we need next in slash proc. With the process id, we can find the memory map of this app process.

This uploads lots of stuff into memory and if you look through it, you will find lip binder, the native code library implementing the interaction

with the binder driver, which allows the app to send data to other app process such as the intent when we call start activity.

Luckily we don't really have to interact with any of the low level stuff directly. That's the beauty of Android and the development SDK.

So we can come back up into the Java application code and just imagine that with start activity, we send this intent object to the target app

and adjust magically works.
### References
[Intent Attack Surface](https://app.hextree.io/courses/intent-threat-surface/intents-and-activities)
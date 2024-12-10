
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

### Incoming Intent

The intent object that was used to start an activity, is available to the app via `getIntent()`.This feature is used to pass data to other apps, and thus it becomes a major attack surface.
#### Intent Attack surface

- `Activity.getIntent()`
- `onActivityResult(int requestCode, int resultCode, Intent data)`
- `onNewIntent(Intent data)`

we can add things to an intent by using `intent.putExtra("hextree",1337);`

### References
[Intent Attack Surface](https://app.hextree.io/courses/intent-threat-surface/intents-and-activities)
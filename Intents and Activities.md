
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


### References
[Intent Attack Surface](https://app.hextree.io/courses/intent-threat-surface/intents-and-activities)
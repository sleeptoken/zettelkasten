
2025-06-25 12:22

Source: #android 
## Exported vs. Non-Exported Components

When looking at the threat surface of an app, specifically the activities, the broadcast receivers and services. So far, we only looked at whether they are exported or not. Non-exported activities, for example, cannot be directly started from another app. They are essentially just private activities used by the app internally.
So from the attacker perspective, we can generally ignore them unless we can find an issue such as an intent redirect or get a hold of a pending intent. 

This means as a developer, not exporting components is one of the strongest protections you can implement. This is real attack surface reduction. So if it doesn't have to be exported, always set exported to false.

And if you are a developer, it's important that you pay attention to this and review your manifest. Because when you add new features, for example, if you create a broadcast receiver, by default it will be exported. So these mistakes could happen. 
## Normal System Permissions

If you want to connect to some kind of web API to fetch some data from the internet, your app has to include a tag in the Android manifest, declaring that you want to use the `android.permission.INTERNET`, and maybe `android.permission.ACCESS_NETWORK_STATE`. These are so-called normal-level permissions and are granted automatically when the app is installed. 

> "Normal" is the default value. These permissions give access to features that have minimal risk to other applications, the system, or the user. The system automatically grants these types of permissions to a requesting application at installation, without asking for the user's explicit approval.

#### Why declare permissions?

yes, it might seem useless, but it adds visibility and transparency to what apps are doing. An app without the INTERNET permission cannot create a network socket. So if the app wants to do internet stuff, they cannot hide it. They must declare this permission. 

#### Android core source code (permissions)

So when a vendor like Google builds their Android image, this file decides how the permissions behave. 

the `android:protectionLevel="normal"`, which means regular apps can get this permission simply by adding a `<user-permission>` tag in their Android  manifest. 

### References
[Android Permissions](https://app.hextree.io/courses/android-permissions/permission-overview)
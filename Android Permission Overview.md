
2025-06-25 12:22

Source: #android 
## Exported vs. Non-Exported Components

When looking at the threat surface of an app, specifically the activities, the broadcast receivers and services. So far, we only looked at whether they are exported or not. Non-exported activities, for example, cannot be directly started from another app. They are essentially just private activities used by the app internally.
So from the attacker perspective, we can generally ignore them unless we can find an issue such as an intent redirect or get a hold of a pending intent. 

This means as a developer, not exporting components is one of the strongest protections you can implement. This is real attack surface reduction. So if it doesn't have to be exported, always set exported to false.

And if you are a developer, it's important that you pay attention to this and review your manifest. Because when you add new features, for example, if you create a broadcast receiver, by default it will be exported. So these mistakes could happen. 
## Normal System Permissions

If you want to connect to some kind of web API to fetch some data from the internet, your app has to include a tag in the Android manifest, declaring that you want to use the `android.permission.INTERNET`, and maybe `android.permission.ACCESS_NETWORK_STATE`. These are so-called normal-level permissions and are granted automatically when the app is installed. "Normal" is the default value.

These permissions give access to features that have minimal risk to other applications, the system, or the user. The system automatically grants these

types of permissions to a requesting application at installation, without asking for the user's explicit approval, though the user always has the option to review

these permissions before installing. You might wonder why do you even have to declare them when you just get them without requiring additional consent? You just declare it, and upon installation, Android grants you those permissions just like that. And yes, it might seem useless,

but it adds visibility and transparency to what apps are doing. An app without the INTERNET permission cannot create a network socket.

So if the app wants to do internet stuff, they cannot hide it. They must declare this permission. So it's really helpful,

especially for the Play Store, to tell you about the capabilities of some apps. And for us as attackers, the more permissions an app requires,

probably the more interesting attack surface it has as well. If you look at the Android core source code for your specific Android version,

you can find this Android manifest. This contains, I think, all default permissions that exist, including comments, which is really

great if you don't understand one. In here you basically have the counterpart to the that we declare in our own manifest.

So when a vendor like Google builds their Android image, this file decides how the permissions behave. We will get into all the different protection levels and some other quirks soon. For now, let's just focus on the normal protection level like the INTERNET permission has.

So in this Android version, it was decided that the INTERNET permission that allows applications to open network sockets has the permission level normal,

which means regular apps can get this permission simply by adding a permission> tag in their Android manifest. There's another special case for

so-called instant apps, but it's not that important unless you are developing an instant app yourself. Anyway, let's think generally as attackers.

For us, it's mostly important to know which permissions can we assume our attacking app can request for, and it still be a valid attack model?

It doesn't make sense to assume that our attacker app has root permissions, for example. Unfortunately, it's not super easy to

Fanswer which permissions constitute still a valid attack model. It always depends on the context, but as a rule of thumb, any normal

permission is probably fair game. You can assume that as an attacker, you can use any permission in here that




### References
[Android Permissions](https://app.hextree.io/courses/android-permissions/permission-overview)
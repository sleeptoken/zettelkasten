
2025-06-25 12:22

Source: #android 

There exist several [`protectionLevel`](https://developer.android.com/guide/topics/manifest/permission-element) for permissions

> [!quote]
> Eighty percent of success is showing up.
> © Woody Allen

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

Also have a look at the Android core [AndroidManifest.xml](https://android.googlesource.com/platform/frameworks/base.git/+/refs/heads/main/core/res/AndroidManifest.xml) for a huge list of default permissions.
So when a vendor like Google builds their Android image, this file decides how the permissions behave. 

the `android:protectionLevel="normal"`, which means regular apps can get this permission simply by adding a `<user-permission>` tag in their Android  manifest. 

## Dangerous System Permissions

Dangerous means it's a higher risk permission that gives a requesting application access to either private user data, or control over the device that can negatively impact the user

Let's say you want to access the user's contacts, the address book. Then the permission we have to request is `android.permission.READ_CONTACTS` which is considered a dangerous permission. 
- In old Android version 5.1, declaring it like this was already enough. This used to trigger a dialog upon installation of the app, asking the user whether it's okay that the app has access to the contact details or not. 
- But this was changed in Android 6. Simply requesting the permission in the manifest is not enough anymore. They have to be dynamically requested afterwards as well.

We can then check whether a certain permission has been granted with `checkselfpermission` 
```java
Log.i("Test","CONTACTS access? "+checkSelfPermission("android.permission.READ_CONTACTS"))
```

If we now run the app and look at the logs, we see a -1, which indicates we are not allowed to read the contacts yet. We have to request it first. So let's call `requestPermissions` with the list of the permissions we want to get, 

```java
if(checkSelfPermission("android.permission.READ_CONTACTS") == -1){
	requestPermissions(new String[]{"android.permission.READ_CONTACTS"},42);
}
```
and that will trigger the dialog asking the user whether the app is allowed to access the contacts or not. 

If you now open the app and click allow, you can see in the logs that we now get a 0 as the return of `checkSelfPermission`, and that means access was granted. Now theoretically we are allowed to access the contacts. 

> First of all, you cannot request permissions that have not been declared previously in the Android manifest. Again, this helps with transparency. An app cannot ever request reading the contacts if it was not transparently mentioned before.

The `RequestPermission` function actually works with intents. So you send this intent to the package manager, and the package manager is then responsible for showing the dialog and of course then handles the response by the user.

## Security Boundaries of Permissions

As you know, for hacking other apps, we usually develop a proof-of-concept app. And so for us, it's important to know

where are the security boundaries? Can we assume our attacker app has certain permissions? Or should a malicious app by definition

not request any permissions? This is not super easy to answer as it depends a lot on the target. But generally, there's one

big rule you should keep in mind. The fewer permissions your attacking app requires, the better. I think this becomes super clear once

we think about an extreme example. There exist very privileged permissions an app could theoretically have that basically allows an app to do anything

like manage your whole device. There are even rooted devices with apps having root access. So if your attacking app

requires root to exploit another app, that doesn't make much sense. Root is basically the highest access you can have, so there is no security boundary

that you cross when you attack an app. So generally speaking, you always want to attack upwards. If your app requires a lot less important permissions than your target, then you'll probably have a valid issue. Let's do an example. Let's imagine there is a very

privileged device managing app that holds INSTALL_PACKAGES permission and thus is allowed to install other apps. And now this app implements and exports an

activity, InstallAppActivity that takes an Intent containing a link to an APK, downloads it, and installs it. Now you can develop a malicious attack

app without requesting any permissions, and just send such an Intent to that other app. And you can force install an APK that is

a vulnerability because your app didn't have the INSTALL PACKAGES permission, but you abused the other app to do it for you.

You could call this a privilege escalation, or some people say this is hijacking the INSTALL_PACKAGES permission from the other app.

But honestly, I don't like the word hijack in this context. Now, let's say there's some quirk in this InstallAppActivity and our attacker app, in order to exploit this, now requires relatively privileged permissions like MANAGE_EXTERNAL_STORAGE for itself to be able to fully access the file system.

And in order to get this permission, the user has to intentionally go into the settings to enable it for this app.

So you really cannot assume that your attacker app easily gets this permission. But there's an argument to be made that

INSTALL_PACKAGES is a way worse. Permission. So the permission that you require is a lot less. And so you can argue that this is

probably still a fair requirement. The severity is not as high as it would be if you didn't require this permission.

It's really limiting the impact and the severity. But there's still a significant jump from just file access to installing arbitrary

APKS. So it should be still a valid issue. So when you hunt for exposed components like exported activities or so,

and they require your app to have a certain permission, always make sure that you attack upwards. Your app should require permissions
### References
[Android Permissions](https://app.hextree.io/courses/android-permissions/permission-overview)
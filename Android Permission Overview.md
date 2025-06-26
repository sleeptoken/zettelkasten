
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

If you now open the app and click allow close the app and run the main activity again, you can see in the logs that we now get

a 0 as the return of checkSelfPermission, and that means access was granted. Now theoretically we are allowed to access the contacts. Two fun facts 


> First of all, you cannot request permissions that have not been declared previously in the Android manifest. Again, this helps with transparency. An app cannot ever request reading the contacts if it was not transparently mentioned before.

The `RequestPermission` function actually works with intents. So you send this intent to the package manager, and the package manager is then responsible for showing the dialog and of course then handles the response by the user.

### References
[Android Permissions](https://app.hextree.io/courses/android-permissions/permission-overview)
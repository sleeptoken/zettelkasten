
2025-06-27 10:59

Source: #android 

Tags: [[Permissions]]

## Identify Valuable Targets via Permissions

If you look around the core Android manifest with all the declared permissions, you might have also noticed a few other protection levels, such as `internal`, `system`, `appop` or `preinstalled`, and there are several more. But generally, as an attacker, these permissions are not super interesting to us and how they work. These are permissions that regular apps cannot get. 

when you are researching system applications like applications that come pre-installed on a phone, when you buy it, these vendor apps, they often use them. And if you find bugs in those apps and you can somehow abuse their permissions indirectly, those are serious issues, probably affecting every phone, and you get CVEs for that.

In a fresh emulator without Play Services, this doesn't even have the Google stuff. There are about 150 apks pre installed 

We can use ADB pull to download the settings APK and throw it into jadx, and the first look should always be the Android manifest. 
- And here we can see it requests tons of permissions and lots of these permissions are privileged system permissions. Only system apps can get these permissions. 
- So this makes the Settings app a very valuable and critical target. And it even has several exported activities that we could try to attack. But many of them also are protected by a permission. 

## Protecting Components with Permissions

Even when an activity is exported, it could still be protected by an additional permission. This is the permission the calling app needs to have in order to be able to actually call this. 

A permission that shows up in quite a lot of apps is, for example, the `android.permission.BIND_JOB_SERVICE` 
allows system apps to bind to an application's task service. 
- These services are often exported, but this permission is involved in the `WorkManager` feature. It can be used by apps to schedule a task or a job sometime in the future. For example, execute something every hour. 
- So you can register the job with the system, and then the system takes care of the scheduling.
- And when your job is supposed to run, the system will connect to your exported service, which is allowed to connect to because it has the system permission `BIND_JOB_SERVICE` and thus it executes your job code.

> [!important]
> So when you see a component protected with the permission, always check what kind of permission it is. Is it a system permission that an attacker app cannot get, or is it a normal permission? You can easily request 

##### Check what kind of permission 

1. A first check can always be the [Android core manifest](https://android.googlesource.com/platform/frameworks/base.git/+/refs/heads/main/core/res/AndroidManifest.xml)
2. Another quick trick for checking is using Android Studio. You can go into the Android manifest of your proof of concept app and try to use that permission. Then Android Studio can show you whether you can get this permission or not,
	- But you should also test it dynamically on your actual device, because some Android vendors might change something weird with permissions, create their own permissions, and not properly declare them, or simply forget to declare it.
	- So, especially for unique vendor permissions or new system permissions, check how it actually behaves on a running phone. 

#### Cool Weather app case study 

Now, besides these system permissions that you declare in your component so That another system application can access your exported component. There's another reason why some developers like to use them. Let's say you develop a weather app that has access to the user's GPS location to show the local weather. 

```xml
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>
```

To do so, the app requests the location permission and the user grants it. Now, what if the app wants to export a service for other apps to read the weather status, and that weather status contains the location?

```xml
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
<service 
	android:name=".WeatherService"
	android:exported="true" android:enabled="true" />
```

Well, that could be a vulnerability because now a malicious app without GPS permissions can query the weather app to get indirectly the user's location. 

So in order to mitigate this attack, the weather app could add the location permission as a requirement to the exported service. 

```xml
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
<service 
	android:name=".WeatherService"
	android:exported="true" android:enabled="true" 
	android:permission="android.permission.ACCESS_FINE_LOCATION"/>
```

With that change, only apps that also were granted access to the location are allowed to use the service to get the weather report, which contains the location details.

## Creating Custom Permissions

So far, we've only seen these official permissions getting declared by this Android core AndroidManifest to protect various areas of the Android system,

but apps can actually create their own custom permissions as well and use the permission levels: normal, dangerous or signature to protect

their exported components such as services and activities. Look at the Termux app because I think it's a perfect example for this.

This is a terminal emulator giving you access to a Linux shell, and of course you can run arbitrary commands in here.

Now, if you look at the manifest of this app, you can see that the app requests some permissions using permissions.

But that's not all. It also contains a permission tag like we have seen the system uses to create these default Android permissions.

This custom permission is called run command and is considered a dangerous permission. If we now look where this permission is involved, we can find an exported service com.termux app-run-command service and from the Termux wiki. We can also learn how to use the

service from our own app to execute commands and think about this. Exposing this kind of functionality could be quite bad, right?

It's remote code execution by design if it wasn't protected with the run command permission. So yes, you can use this Termux

service to execute commands, but you need to have this permission. So let's go ahead and declare that we want to use this

permission in our AndroidManifest. And then in the main activity we can check if our app already has this permission,

and if not, we should request this permission. As you can see, this is exactly the same way how we have requested access

to dangerous system permissions. But this is not a system permission. This is a new custom permission created by the Termux app.

And so if we run this now, we actually get a consent dialog like we have seen with the system permissions. Do we allow our app to execute arbitrary commands within the Termux environment and access files, and this makes all the difference.

Why the exported run command service of the Termux app is not a security issue. It's property protected, and if an app

wants to run a command thanks to the dangerous permission, the system will ask the user whether that is okay or not.

##### Protection Levels

- So far we have only seen dangerous which triggers the special consent dialog. 
- Normal means that it behaves like the normal system permissions.
	- You request them in the `AndroidManifest` and they are granted on install. No special consent screen. 
- Now when you see a permission that is a signature permission, it generally means game over for us as attackers, because you only get this permission if your app is signed with the same key as the app that declared it 
	- Usually our attacker apps are not signed with the same key, so we cannot get this permission. The reason why apps use signature permission is also pretty obvious.
	- It's especially used for cases where multiple apps by the same developer interact with each other. So you create a signature permission and they can talk to each other. But an external app cannot reach the same components. 
	 
But there's somewhat a typical mistake that can happen. And that is when only one app declares the signature permission. So if the user only installs one of the secondary apps that uses the permission but doesn't declare it, then an attacker app has no problem to declare the same permission and attack the secondary app. So when you encounter a signature permission, check other apps by the same developers, whether they are using the permissions and whether they also declare it in those apps as well.

Now, if you have a very creative hacker mind, you might have wondered about the installation order. Sure, when Termux is installed first

and you install your app later and request this permission, the system will see it's a dangerous permission and prompt the user.

But what if our app was installed before Termux? Let's uninstall Termux again and then declare the permission in our own app

as a normal permission. If we run our app now, we can see that we now have the run command permission

without showing a user dialog. So can we now install Termux and then run arbitrary commands? If we try to do that,

the installation of Termux actually fails. Install failed: duplicate permission. The package Termux attempted to redeclare permission run command, which is already owned by our attack app.

So you cannot easily trick the system that way. This is how the integrity of the permissions is ensured.

### References
[[Android Permission Overview]]
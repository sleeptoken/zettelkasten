
2025-07-03 12:30

Source: #android 
## Basic Broadcast Receivers

As an example, we're gonna use `AntennaPod`, which is a podcast app because it contains a lot of great examples involving broadcast receivers.

There are two ways how [broadcast receivers](https://developer.android.com/reference/android/content/BroadcastReceiver) could be used by an app. 
1. First they could be exported via the `AndroidManifest.xml` with a `<receiver>` tag. And like with activities, we generally only care about the exported receivers because they can be reached by other apps. 
2. The other way is by dynamically registering a receiver class using [`registerReceiver()`](https://developer.android.com/reference/android/content/Context#registerReceiver\(android.content.BroadcastReceiver,%20android.content.IntentFilter\)).

#### Dynamically create receivers

Unfortunately, the Android manifest is not the only place where receivers can be created. An app can also dynamically create them at runtime in the code using `registerReceiver`, and can also unregister it again with `unregisterReceiver`.

So if we want to look for that threat surface, we can search for that method in the podcast app. And we will get tons of results. But most of them are in Android library code. This is because a lot of internal Android functionality relies on broadcast receivers as well.
- But our target is the app itself, so we keep focusing on anything that is declared in the `AntennaPod` package path.

> [!tip]
> So when reviewing the broadcasting threat surface, always make sure to check the manifest and look for dynamically registered receivers.

## Sending Broadcasts

First, let's prepare the intent.
```java
Intent intent = new Intent();
intent.setAction("de.danoeh.antennapdsp.intent.SP_APPS_QUERY_FEEDS_RESPONSE");
String[] feedUrls = {"https://media.rss.com/ctbbpodcast/feed.xml"};
intent.putExtra("feeds",feedUrls);
sendBroadcast(intent);
```

And we could, for example, add the excellent Critical Thinking Bug Bounty podcast to it. And that's basically it. 
- We can now call sendBroadcast with the intent very similar to startActivity that we have seen before, except that startActivity launched the app Ul while this happens silently in the background.

However, if we try this now, nothing appears to happen using the flag debug log resolution,
```java
intent.addFlags(Intent.FLAG_DEBUG_LOG_RESOLUTION);
```

we can see our intent is matching the broadcast receiver of the podcast app, but we get an error. Background execution not allowed.
- This is a change that happened in Android 8, which restricts the delivery of implicit broadcasts to apps in order to save battery. But we have an easy way around it.
	- And that is turning this implicit broadcast without a specific target into an explicit broadcast by specifying exactly the target.
```java
intent.setClassName("de.danoeh.antennapod","de.danoeh.antennapod.spa.SPARecceiver");
```

By doing that, the system is okay to deliver it for us, because it doesn't have to wake up potentially hundreds of apps. It just targets this one app.

## System Event Broadcasts

One of the original ideas behind the broadcast receivers is for the system to broadcast events to all interested apps. 
- for example, one of the common ones used is the `BOOT_COMPLETED` broadcast, which is sent out when the phone is started. Your app can register it and execute some code in the receiver when the phone is started. Kind of like an auto start. 

### Trying to exploit this 

It should be obvious that we cannot just send an implicit broadcast with this action to all apps. We are not the system, and when we try it, we get a security exception. Permission denial not allowed to send the broadcast 
```java
Intent intent = new Intent();
intent.setAction("android.intent.action.ACTION_POWER_CONNECTED");
sendBroadcast(intent);
```

And even when turning that into an explicit intent, by directly specifying the target app and the class, the system will still not allow it. 
```java
Intent intent = new Intent();
intent.setAction("android.intent.action.ACTION_POWER_CONNECTED");
intent.setClassName("de.danoeh.antennapod","de.danoeh.antennapod.net.download.service.PowerConnected");
sendBroadcast(intent);
```
This is a protected broadcast

but let's remove the action or use a different action string and still target exactly this class and then try again. We don't get an error,
```java
Intent intent = new Intent();
intent.setAction("test");
intent.setClassName("de.danoeh.antennapod","de.danoeh.antennapod.net.download.service.PowerConnected");
sendBroadcast(intent);
```

and if we look at the logs of the podcast app, we can see we get the log output not charging anymore, but the user allows auto download.
```java
if ("android.intent.action.ACTION_POWER_CONNECTED".equals(action)) {
    Log.d(TAG, "charging, starting auto-download");
    AutoDownloadManager.getInstance().autodownloadUndownloadedItems(context);
} else if (!UserPreferences.isEnableAutodownloadOnBattery()) {
    Log.d(TAG, "not charging anymore, canceling auto-download");
    DownloadServiceInterface.get().cancelAll(context);
} else {
    Log.d(TAG, "not charging anymore, but the user allows auto-download when on battery so we'll keep going");
}
```
This means we cannot get into the if case here. We cannot set the protected system broadcast action value. But the else case is not specifically checking the action, so we can still reach it with our explicit broadcast and invoke this broadcast receiver.

## Intercept and Redirecting Broadcasts



### References
[Broadcast Receivers](https://app.hextree.io/courses/broadcast-receivers/broadcast-threat-surface)
https://gaberoy.zip/posts/android-broadcast/android-broadcast/#what-is-a-broadcast-receiver 
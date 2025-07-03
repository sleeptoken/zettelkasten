
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




### References
[Broadcast Receivers](https://app.hextree.io/courses/broadcast-receivers/broadcast-threat-surface)
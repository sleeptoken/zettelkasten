
2025-06-25 12:22

Source: #android 
## Exported vs. Non-Exported Components

When looking at the threat surface of an app, specifically the activities, the broadcast receivers and services. So far, we only looked at whether they are exported or not. Non-exported activities, for example, cannot be directly started from another app. They are essentially just private activities used by the app internally.
So from the attacker perspective, we can generally ignore them unless we can find an issue such as an intent redirect or get a hold of a pending intent. 
.
This means as a developer, not exporting components is one of the strongest protections you can implement. This is real attack surface reduction. So if it doesn't have to be exported, always set exported to false.

And if you are a developer, it's important that you pay attention to this and review your manifest. Because when you add new features, for example, if you create a broadcast receiver, by default it will be exported. So these mistakes could happen. But export the true or false is not

the only way to protect the exposed threat surface. There are cases where an app wants to export something but doesn't want to

### References
[Android Permissions](https://app.hextree.io/courses/android-permissions/permission-overview)
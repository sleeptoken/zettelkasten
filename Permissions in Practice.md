
2025-06-27 10:59

Source: #android 

Tags: [[Permissions]]

## Identify Valuable Targets via Permissions

If you look around the core Android manifest with all the declared permissions, you might have also noticed a few other protection levels,

such as intermal system, app-op or preinstalled, and there are several more. But generally, as an attacker, these permissions are not super

interesting to us and how they work. These are permissions that regular apps cannot get. So if we hunt for bugs in apps from

the App Store or we develop our own attack apps, we usually don't really get in touch with them. But when you are researching system

applications like applications that come pre-installed on a phone, when you buy it, these vendor apps, they often use them.

And if you find bugs in those apps and you can somehow abuse their permissions indirectly, those are serious issues,

probably affecting every phone, and you get CVEs for that. And I want to highlight just because maybe it's not clear, there are many probably affecting every phone, and you get CVEs for that And I want to highlight just because maybe it's not clear, there are many

more apps pre-installed on a clean phone than the few apps you can see in the launcher menu on this fresh emulator,

for example, without Play Services. So this doesn't even have the Google stuff. There seems to be around a dozen apps,

but if you go ADB shell and list all packages that are actually installed on the emulator, there are over 150 APKS.

G You can, for example, see here your own develop test apps being stored in data app. But there are system apps installed in

all these other privileged locations, such as system. And these apps might use these very privileged system permissions. For example,

here is the Android Settings app. Yes, the Android settings is just another app, which means it's a possible target to attack as well. here is the Android Settings app. Yes, the Android settings is just another app, which means it's a possible target to attack as well.

We can use ADB pull to download the APK and throw it into git, and the first look should always be the Android manifest.

And here we can see it requests tons of permissions and lots of these permissions are privileged system permissions. Only system apps can get

these permissions. So this makes the Settings app a very valuable and critical target. And it even has several exported

activities that we could try to attack. But many of them also are protected by a permission. And so let's talk about that concept next.

### References
[[Android Permission Overview]]
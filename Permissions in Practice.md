
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



### References
[[Android Permission Overview]]
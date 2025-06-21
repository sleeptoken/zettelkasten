
2025-06-19 16:44

Source: #android 

Web links that result into opening an app are so called [deep links](https://developer.android.com/training/app-links/deep-linking).
## Browser-to-App Attack Surface

They basically connect the world of websites with the world of mobile apps by allowing websites to launch apps.
You can easily recognize whether an app can handle deep links from the Android manifest. By looking for intent filters with the Browsable category
```xml
<intent-filter>
	<category android:name="android.intent.category.BROWSABLE"/>
</intent-filter>
```
they are allowed to be browsed from a web browser, and usually they also specify one or more data tags
if we click a link that opens our app. our link click is sent as an intent to our app. we can see more information in the debug dialog 

Generally, deep links are interesting because they increase the attack surface of activities from just app to app communication to the whole web.
- If a user clicks on an evil link and that could trigger this activity, the information the website can pass along in the intent to the target app is much more limited than in an app to app communication using `startActivity`.
- But if there is an issue, it's automatically a lot more critical because you can reach it from a website.

Exposing activities through a deep link could happen accidentally. Also some developers intended the exported activities just to be called by a link which is very limited, but these exposed activities can still be called by other apps as well.



### References
[Intent Attack Surface](https://app.hextree.io/courses/intent-threat-surface/android-deep-links)
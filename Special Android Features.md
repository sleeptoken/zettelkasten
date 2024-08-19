
2024-08-17 18:41

Source: #android 

Tags: [[intent]]

[[Building Android POC app]] sequel  
### Sending Intents

add the below code after the `counter++;` inside an `if(counter==10)` statement
and after clicking on the button 10 times the app opens the browser and navigates to the URL we specified 

``` java
Intent browserIntent = new Intent(Intent.ACTION_VIEW, Uri.parse("https://hextree.io/"));
startActivity(browserIntent); 
```

Activity creates the UI we see and so its farfetched to think that start activity will launch and display such an activity, but which activity will be shown depends on the parameter intent
what do we intend to open ? 
in the above code we intended to VIEW something and the data we pass is the URL 
#### Gist 

1. we created an intent to view a URL and we start activity 
2. our app passes the intent to the android OS 
3. which now looks at the intent, sees that we want to view a URL, and android will start looking for installed apps that can fulfil our request, 
4. it'll find chrome browser app and hand over the intent to that. 
5. but how does the OS know that chrome can handle this? 
6. well it has to do with the android manifest 
7. in the android manifest file of chrome in activity-alias -> intent filter which says that it can handle actions to view the following protocols example. http, https etc. 
### Receiving Intents





### References

android manifest of chrome - [chrome/android/java/AndroidManifest.xml - chromium/src - Git at Google (googlesource.com)](https://chromium.googlesource.com/chromium/src/+/b71e98cdf14f18cb967a73857826f6e8c568cea0/chrome/android/java/AndroidManifest.xml#156)